---
layout: post
title: Computing the turbulent kinetic energy spectrum
date: 2025-08-11 15:00:00-0500
description: 
tags: computational-fluid-dynamics direct-numerical-simulation turbulence dataset kolmogorov
toc:
  sidebar: left
background: '#FFFFFF' 
color: black
---

## Introduction
I find it difficult to understand the subtleties around the 3D/2D/1D turbulent kinetic energy spectra, and exactly what [Kolmogorov](https://www.ams.jhu.edu/~eyink/Turbulence/classics/Kolmogorov41a.pdf) and [Pope](https://elmoukrie.com/wp-content/uploads/2022/04/pope-s.b.-turbulent-flows-cambridge-university-press-2000.pdf) are talking about. I'm writing this post for my own sanity and clarity. I hope you find it useful too.

## The dataset
See my blog post [here](https://ryleymcconkey.com/2025/07/HIT-turbulence-dataset/) on the 3D homogenenous isotropic turbulence dataset I'm going to use for this post. All code for this blog post is available on [github](https://github.com/rmcconke/HIT3D_dataset).

## The basic overview

We start with an instantaneous velocity vector field:
$$
\vec{u}(\vec{x}) = (u_x(\vec{x}), u_y(\vec{x}), u_z(\vec{x})) \ . 
$$
In physical space, the position vector is $\vec{x}$.


```
data = load_box_timeseries(prefix = 'forced_boxfilter_fs8_highres', box_number=0)[0:10,:,:,:]
print(f'Velocity field shape u(t,u,x,y,z): {data.shape}')

Velocity field shape u(t,u,x,y,z): (10, 3, 64, 64, 64)
```

Visualizing a 3D vector field is hard. Let's try to visualize $\vec{u}(\vec{x})$:

### Spectral velocity 
We take the Fourier transform of this field to get the *spectral velocity*:
$$
\vec{\hat{u}}(\vec{k}) = (\hat{u}_x(\vec{k}), \hat{u}_y(\vec{k}), \hat{u}_z(\vec{k})) \ . 
$$
This is also a vector field, but in wave-space. The position vector in wave-space is the *wave-vector* $\hat{k}$. The wave-vector is $\vec{k} = (k_x,k_y,k_z)$. In wave-space, the wave-vector points to fluctuations of a certain wavelength (given by $\lambda = 2\pi/|\vec{k}|$) in a certain direction (given by the unit vector $\vec{k}/|\vec{k}|$). For example, $\vec{k} = (k_x,0,0)$ corresponds with motions along the $x$ direction.

```
uhat = np.fft.fftn(data,axes=(2, 3, 4), norm="forward") # use forward (divide by N) so Parseval's theorem holds
print(f'Spectral velocity shape uhat(t,uhat,k_x,k_y,k_z): {uhat.shape}')

Spectral velocity shape uhat(t,uhat,k_x,k_y,k_z): (10, 3, 64, 64, 64)
```

Let's try to visualize this 3D vector field $\vec{\hat{u}}(\vec{k})$:

### Velocity spectrum tensor
From our spectral velocity, we can derive the following second-order tensor, called the *velocity spectrum tensor*:
$$
\Phi_{ij}(\vec{k}) = \frac{1}{V}\left(\vec{\hat{u}}_i(\vec{k}) \vec{\hat{u}}_j(\vec{k}))\right)
$$
*Note: the above formula assumes homogeneity of the field*.

This is a second-order tensor field in wave-space. It represents the distribution of kinetic energy between the $i$ and $j$ velocity components. Pope says 
*"it represents the Reynolds-stress density in wavenumber space: that is, $\Phi_{ij}(\vec{k})$ is the contribution (per unit volume in wavenumber space) from the Fourier mode $e^{i\vec{k}\cdot\vec{x}}$ to the Reynolds stress $\overline{u_iu_j}$."* 

```
Phi_timeseries = V * np.einsum('tixyz,tjxyz->tijxyz', uhat, np.conj(uhat), optimize=True) # Need to multiply by volume for unit consistency
print(f'Velocity spectrum tensor shape Phi(t,phi_i,phi_j,k_x,k_y,k_z): {Phi_timeseries.shape}')

Velocity spectrum tensor shape Phi(t,phi_i,phi_j,k_x,k_y,k_z): (10, 3, 3, 64, 64, 64)
```


It's a cool tensor! Maybe a future blog post is coming :)


### 3D energy spectrum
We can recover the 3D energy spectrum $E(\vec{k})$ via the trace of this tensor:

$$
E(\vec{k}) = \text{tr}\left(\Phi_{ij}(\vec{k}))\right) =  \frac{1}{2}\sum_{i=1}^3 \Phi_{ii}(\vec{k})
$$

This is a scalar-valued function of the wave-vector $\vec{k}$. It's the kinetic energy density along each direction (e.g., $x$, $y$, and $z$). Yes, the kinetic energy spectrum in turbulence is actually a function of all 3 spatial coordinates. Often we see it plotted only against a single scalar wavenumber --- this is the 1D spectrum, discussed below.

### 1D energy spectrum
Up to this point, we've assumed homogeneity (above). But, if the flow is *isotropic* (or we close our eyes and pretend it is), then we can actually come up with a 1D spectrum, which is a function of $|\vec{k}|$ only. 

$$
E(|\vec{k}|) = E(k)= \int_{|\vec{k}| = k} E(\vec{k}')dS(\vec{k}')
$$

The overloading $|\vec{k}|=k$ here is done for brevity, but it's a pretty brutal notation in my opinion. $dS(\vec{k}')$ is around a spherical shell in wave-space. To practically compute this, we just bin our calculated $E(\vec{k})$ by $|\vec{k}|$.

