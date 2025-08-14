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
I find it difficult to understand the subtleties around the 3D/2D/1D turbulent kinetic energy spectra, and exactly what [Kolmogorov](https://www.ams.jhu.edu/~eyink/Turbulence/classics/Kolmogorov41a.pdf) and [Pope](https://elmoukrie.com/wp-content/uploads/2022/04/pope-s.b.-turbulent-flows-cambridge-university-press-2000.pdf) are talking about. I'm writing this post for my own sanity and clarity. I hope you find it useful too. I'm mostly going to write from the point of view of the discrete Fourier transform, since I kind of like making computers do math for me.

Remember, the core idea of the Fourier transform is to decompose a field/signal/thing into a superposition of sine and cosine waves. The Fourier coefficients are $A_k = a+ib$, where $a$ is the cosine amplitude, and $b$ is the sine amplitude. We can store both total wave amplitude and phase using this imaginary number. We index which cosine/sine amplitudes and phases should be included by their frequency index $k$.

See my blog post [here](https://ryleymcconkey.com/2025/07/HIT-turbulence-dataset/) on the 3D homogenenous isotropic turbulence dataset I'm going to use for this post. All code for this blog post is available on [github](https://github.com/rmcconke/HIT3D_dataset).


## On units and scaling in the Fourier transform

Say our original function is in units of [`function units`], and is indexed by [`coordinate units`]. For example, at a fixed time our velocity field has units of m/s, and is indexed by m:

$$
\vec{u}(\vec{x}) \\ [\vec{u}]=\text{m/s} \\ [\vec{x}] = \text{m}
$$

<hr class="remark-start">
<span class="ballet-fancy">Remark.</span>

The Fourier transform takes a function indexed by [`coordinate units`] and returns a function indexed by [`coordinate units`]$^{-1}$. 
<hr class="remark-end">

- I think of [`coordinate units`]$^{-1}$ as *oscillations per coordinate unit*. In Fourier space, we index the Fourier transform by choosing a given number of *oscillations per coordinate unit*. 
  - For example, in our velocity field $\vec{u}(\vec{x})$, we index the motions with a certain number of oscillations/m (AKA cycles/m). 
- It's confusing that we normally write this as $\text{m}^{-1}$, so we don't see the "oscillations" part. 
  - It's even more confusing that when our original function is indexed by time (in seconds), the Fourier transform is indexed by Hz (which is just units of cycles/s), so we don't even directly see the inverse of the original coordinate unit sitting there --- we use an entirely new unit. 
- It's even MORE confusing that sometimes we index the Fourier transform *radians* per coordinate unit rather than cycles. Depending on the convention used in our Fourier tranform, we might end up doing this. 

<hr class="remark-start">
<span class="ballet-fancy">Remark.</span>

The discrete Fourier transform in `numpy` takes a function in units of [`function units`] and returns a function indexed by [`function units`]. 
<hr class="remark-end">

- For example, `np.fft.fftn`  of our velocity field in m/s will return a function with units of m/s. 
  - The interpretation of this unit (m/s) in Fourier space is the amplitude of the plane-wave that is present in our original velocity field.
- Roughly: in the Fourier transform, we index the amplitude and phase of the plane-wave given a frequency.

### The details of units and scaling
Given a velocity field $\vec{u}(\vec{x})$, in [m/s], let's walk through the units.

`numpy` FFT definition, for 1D data:
$$
A_k = \sum_{m=0}^{n-1} a_m e^{-2\pi i \frac{mk}{n}} \ , k =0,...,n-1
$$
- $a_m$: input data at index $m$
- $n$: total number of samples
- $k$: frequency index (a bin in Fourier space)
- $A_k$: Fourier coefficient for bin $k$
- $mk/n$: dot product of sample index and frequency index, normalized by length $n$.

Let's look at it just for the $u$ velocity component ($x$-velocity) in our 3D data. A rough shorthand is

$$
\hat{u}(\vec{k}) = \sum_{\vec{n}} u[\vec{n}] e^{-2\pi i (\vec{n}\cdot \vec{k})/N}
$$ 

Which actually means 

$$
\hat{u}[k_x, k_y, k_z] = \sum_{n_x=0}^{N_x-1} \sum_{n_y=0}^{N_y-1} \sum_{n_z=0}^{N_z-1} u[n_x, n_y, n_z] \, e^{- 2\pi i \left( \frac{n_x k_x}{N_x} + \frac{n_y k_y}{N_y} + \frac{n_z k_z}{N_z} \right)}
$$


- $u[\vec{n}]$: velocity data at grid index vector $\vec{n} = (n_x, n_y, n_z)$.
- $\vec{k}$: integer frequency index vector $(k_x,k_y,k_z)$
- $N$: number of grid points in each direction
- $\left( \frac{n_x k_x}{N_x} + \frac{n_y k_y}{N_y} + \frac{n_z k_z}{N_z} \right)$: a normalized dot product.


The units of `numpy`'s FFT are the same as the original field. You can see in the above that we're just multiplying the original field with a dimensionless number ($e^{\text{stuff}}$) This departs from the units in the definition of the continuous Fourier transform. 

## The basic overview

We start with an instantaneous velocity vector field:
$$
\vec{u}(\vec{x}) = (u_x(\vec{x}), u_y(\vec{x}), u_z(\vec{x})) \ . 
$$
In physical space, the position vector is $\vec{x}$.


```
# Let's load 10 time snapshots of our velocity field
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

Let's try to visualize this 3D vector field $\vec{\hat{u}}(\vec{k})$:

(coming soon)

### Velocity spectrum tensor
From our spectral velocity, we can derive the following second-order tensor, called the *velocity spectrum tensor*:
$$
\Phi_{ij}(\vec{k}) = \frac{1}{V}\left(\vec{\hat{u}}_i(\vec{k}) \vec{\hat{u}}_j(\vec{k}))\right)
$$
*Note: the above formula assumes homogeneity of the field*.

This is a second-order tensor field in wave-space. It represents the distribution of kinetic energy between the $i$ and $j$ velocity components. Pope says 
*"it represents the Reynolds-stress density in wavenumber space: that is, $\Phi_{ij}(\vec{k})$ is the contribution (per unit volume in wavenumber space) from the Fourier mode $e^{i\vec{k}\cdot\vec{x}}$ to the Reynolds stress $\overline{u_iu_j}$."* 

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

The overloading $|\vec{k}|=k$ here is done for brevity, but it's a pretty brutal notation in my opinion. $dS(\vec{k}')$ is around a spherical shell in wave-space. To practically compute this, we just bin our calculated $E(\vec{k})$ by $|\vec{k}|$, as shown below.

(code coming soon)


To-do:
Write alternative method just using fft(u), no velocity spectrum tensor.
Maybe the velocity spectrum tensor stuff should be a separate blog post.
