---
layout: post
title: The Navier-Stokes Rosetta Stone
date: 2023-12-13 18:00:00-0500
description: Several different forms of the Navier-Stokes equations
tags: fluids education math theory
toc:
  sidebar: left
---


## Introduction
I find is much easier to understand a notation type when it's written beside a notation I already know.
In this post, I will give several important equations in fluid mechanics in three forms: vector calculus notation, Einstein notation, and with the Cartesian components written out. The purpose of this post is for education.
Additionally, I start with the more general Cauchy equation and explain when we make certain assumptions to get the compressible and incompressible Navier-Stokes equations.


References I used to compile this post:
1. &Ccedil;engel and Cimbala, *Fluid Mechanics: Fundamentals and Applications*, McGraw Hill, 4th Edition.
2. Pope, *Turbulent Flows*, Cambridge University Press.
3. [Wikipedia Page](https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_equations) on Navier-Stokes Equations


## Prerequisites

### Format
The format is as follows:

Quantity

$$\text{Vector calculus notation}$$

$$\text{Einstein notation}$$

$$\text{Cartesian components}$$

### Tensors

Vector (rank 1 tensor)

$$\vec{a}$$

$$a_i$$

$$\begin{bmatrix} a_x\\ a_y\\ a_z \end{bmatrix}$$

Velocity vector

$$\vec{V}$$

$$v_i$$

$$\begin{bmatrix} u\\ v\\ w \end{bmatrix}$$

Matrix (rank 2 tensor)

$$A$$

$$A_{ij}$$

$$\begin{bmatrix} A_{xx} & A_{xy} & A_{xz}\\ A_{yx} & A_{yy} & A_{yz}\\ A_{zx} & A_{zy} & A_{zz} \end{bmatrix}$$

Identity matrix

$$I$$

$$\delta_{ij}$$

$$\begin{bmatrix} 1 & 0 & 0\\ 0 & 1 & 0\\ 0 & 0 & 1 \end{bmatrix}$$

### Operations

#### Dot product (inner product)

$$\vec{a}\cdot \vec{b}$$

$$a_i b_i$$

$$a_x b_x + a_y b_y + a_z b_z$$

#### Divergence of a vector

$$\vec{\nabla}\cdot \vec{a}$$

$$\frac{\partial a_i}{\partial x_i}$$

$$\frac{\partial a_x}{\partial x} + \frac{\partial a_y}{\partial y} + \frac{\partial a_z}{\partial z}$$

#### Vector-matrix product

$$\vec{a}\cdot B$$

$$a_i B_{ij}$$

$$\begin{bmatrix} a_x B_{xx} & a_yB_{xy} & a_zB_{xz}\\ a_x B_{yx} & a_yB_{yy} & a_zB_{yz}\\ a_x B_{zx} & a_yB_{zy} & a_zB_{zz} \end{bmatrix}$$

#### Divergence of a tensor

$$\vec{\nabla}\cdot A$$

$$\frac{\partial A_{ij}}{\partial x_i}$$

$$\begin{bmatrix} \frac{\partial B_{xx}}{\partial x} + \frac{\partial B_{yx}}{\partial y} + \frac{\partial B_{zx}}{\partial z}\\ \frac{\partial B_{xy}}{\partial x} + \frac{\partial B_{yy}}{\partial y} + \frac{\partial B_{zy}}{\partial z}\\ \frac{\partial B_{xz}}{\partial x} + \frac{\partial B_{yz}}{\partial y} + \frac{\partial B_{zz}}{\partial z} \end{bmatrix}$$

#### Tensor product or outer product

$$\vec{a}\vec{b} \equiv \vec{a}\otimes\vec{b}$$

$$a_i b_j$$

$$\begin{bmatrix} a_x b_x & a_x b_y & a_x b_z \\ a_y b_x & a_y b_y & a_y b_z \\ a_z b_x & a_z b_y & a_z b_z  \end{bmatrix}$$

#### Gradient of a vector

$$\vec{\nabla} \vec{a}$$

$$\frac{\partial a_i}{\partial x_j}$$

$$\begin{bmatrix} \frac{\partial a_x}{\partial x} & \frac{\partial a_x}{\partial y}  & \frac{\partial a_x}{\partial z} \\ \frac{\partial a_y}{\partial x} & \frac{\partial a_y}{\partial y} & \frac{\partial a_y}{\partial z}\\ \frac{\partial a_z}{\partial x} & \frac{\partial a_z}{\partial y} & \frac{\partial a_z}{\partial z}  \end{bmatrix}  $$


## Equations in fluid mechanics

We start with Cauchy's equation, then expand the various terms to arrive at the compressible Navier-Stokes equations. After assuming constant density, we arrive at the incompressible Navier-Stokes equations.

### Important definitions

#### Material derivative of velocity

The material derivative operator is notated as $$\rho D()/Dt$$. The material derivative of velocity appears frequently.

$$\frac{D\vec{V}}{Dt} = \frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}$$

$$\frac{Dv_j}{Dt} = \frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}$$

$$\begin{align*}
\frac{Du}{Dt} &= \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}\\
\frac{Dv}{Dt} &= \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}\\
\frac{Dw}{Dt} &= \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}
\end{align*}$$

#### Stress tensor

The Cauchy stress tensor is made up of isotropic (hydrostatic) stress, from pressure, and anisotropic (deviatoric) stress, from viscous forces.

$$\sigma = -P I + \tau$$

$$\sigma_{ij}= -P\delta_{ij} + \tau_{ij}$$

$$\sigma = -\begin{bmatrix}P & 0 & 0\\ 0 & P & 0\\0 & 0 & P \end{bmatrix} + \begin{bmatrix}\tau_{xx} & \tau_{xy} & \tau_{xz} \\ \tau_{yx} & \tau_{yy} & \tau_{yz} \\ \tau_{zx} & \tau_{zy} & \tau_{zz}\end{bmatrix}$$

#### Viscous stress tensor

This is the constitutive relation describing how shear is converted to a viscous stress. $$\zeta$$ is the second viscosity or bulk viscosity of the fluid, and is usually assumed to be zero. $$\mu$$ is the dynamic viscosity. Shown here is a linear relationship (Newtonian fluid).

$$\tau = \zeta (\vec{\nabla}\cdot \vec{V})I + \mu [\vec{\nabla}\vec{V} + (\vec{\nabla}\vec{V})^\text{T} - \tfrac{2}{3}(\vec{\nabla} \cdot \vec{V})I ]$$

$$\tau_{ij} = \zeta \frac{\partial v_k}{\partial x_k}\delta_{ij} + \mu \left[\frac{\partial v_j}{\partial x_i}  + \frac{\partial v_i}{\partial x_j} - \frac{2}{3}\frac{\partial v_k}{\partial x_k} \delta_{ij}\right]$$

$$\begin{align*}\tau = &\zeta \begin{bmatrix}
    \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 & 0 \\ 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 \\
    0 & 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right)
\end{bmatrix} \\
&+ \mu \begin{bmatrix}
    2\frac{\partial u}{\partial x} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial v}{\partial x} + \frac{\partial u}{\partial y} &  \frac{\partial w}{\partial x} + \frac{\partial u}{\partial z} \\
    \frac{\partial u}{\partial y} + \frac{\partial v}{\partial x}  & 2\frac{\partial v}{\partial y} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial w}{\partial y} + \frac{\partial v}{\partial z}  \\
    \frac{\partial u}{\partial z} + \frac{\partial w}{\partial x}  & \frac{\partial v}{\partial z} + \frac{\partial w}{\partial y} & 2\frac{\partial w}{\partial z} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right)
\end{bmatrix}
\end{align*}$$

### Compressible continuity equation

$$\frac{\partial \rho}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V}) = 0$$

$$\frac{\partial \rho}{\partial t} + \frac{\partial (\rho v_j)}{\partial x_j} = 0$$

$$\frac{\partial \rho}{\partial t} + \frac{\partial(\rho u)}{\partial x} + \frac{\partial (\rho v)}{\partial y} + \frac{\partial (\rho w)}{\partial z} = 0$$

### Incompressible continuity equation

Assumes $$\rho$$ is constant.

$$\vec{\nabla} \cdot \vec{V} = 0$$

$$\frac{\partial v_j}{\partial x_j} = 0$$

$$\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z} = 0$$

### Cauchy's equation

General equation for momentum transport in a continuum. Here, we consider gravity as the only body force.

$$\frac{\partial (\rho \vec{V})}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V} \vec{V}) = \rho \vec{g} + \vec{\nabla} \cdot \sigma$$

$$\frac{\partial (\rho v_j)}{\partial t} + \frac{\partial }{\partial x_i}(\rho v_j v_i) = \rho g_j + \frac{\partial \sigma_{ij}}{\partial x_i}$$

$$\begin{align*}
\frac{\partial(\rho u)}{\partial t} + \frac{\partial(\rho uu)}{\partial x} + \frac{\partial (\rho uv)}{\partial y} + \frac{\partial (\rho uw)}{\partial z} &= \rho g_x + \frac{\partial \sigma_{xx}}{\partial x} + \frac{\sigma_{yx}}{\partial y} + \frac{\sigma_{zx}}{\partial z}\\
\frac{\partial (\rho v)}{\partial t } + \frac{\partial (\rho u v)}{\partial x } + \frac{\partial (\rho v v)}{\partial y} + \frac{\partial (\rho v w)}{\partial z}&= \rho g_y + \frac{\partial \sigma_{xy}}{\partial x} + \frac{\sigma_{yy}}{\partial y} + \frac{\sigma_{zy}}{\partial z}\\
\frac{\partial (\rho w)}{\partial t} + \frac{\partial (\rho u w)}{\partial x} + \frac{\partial(\rho v w)}{\partial y} + \frac{\partial (\rho w w)}{\partial z}&= \rho g_z + \frac{\partial \sigma_{xz}}{\partial x} + \frac{\sigma_{yz}}{\partial y} + \frac{\sigma_{zz}}{\partial z}
\end{align*}$$

### Cauchy's equation - expanded

The left-hand side of the above equation can be replaced, and the Cauchy stress tensor expanded into isotropic and anisotropic components as follows. We haven't made any major assumptions yet.

$$\rho\frac{D\vec{V}}{Dt} = - \vec{\nabla} P + \vec{\nabla} \cdot \tau + \rho \vec{g}$$

$$\rho \frac{D v_j}{Dt} = -\frac{\partial P}{\partial x_j} + \frac{\partial \tau_{ij}}{\partial x_i} + \rho g_j$$

$$\begin{align*}
\rho \frac{D  u}{Dt}&= - \frac{\partial P}{\partial x} + \frac{\partial \tau_{xx}}{\partial x}+ \frac{\partial \tau_{yx}}{\partial y}+ \frac{\partial \tau_{zx}}{\partial z} + \rho g_x\\
\rho \frac{D v}{Dt}&= - \frac{\partial P}{\partial y} + \frac{\partial \tau_{xy}}{\partial x}+ \frac{\partial \tau_{yy}}{\partial y}+ \frac{\partial \tau_{zy}}{\partial z} + \rho g_y\\
\rho \frac{D  w}{Dt}&= - \frac{\partial P}{\partial z} + \frac{\partial \tau_{xz}}{\partial x}+ \frac{\partial \tau_{yz}}{\partial y}+ \frac{\partial \tau_{zz}}{\partial z} + \rho g_z\\
\end{align*}$$

### Compressible Navier-Stokes equations

Assuming $$\mu$$ is constant, and a Newtonian fluid, we get the compressible Navier-Stokes equations:

$$\rho \frac{D\vec{V}}{Dt} = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \tfrac{1}{3}\mu \vec{\nabla}(\vec{\nabla}\cdot\vec{V}) + \rho \vec{g}$$


$$\rho \frac{D v_j}{Dt} = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \frac{1}{3}\mu \frac{\partial^2 v_i}{\partial x_j \partial x_i} + \rho g_j$$

$$
\begin{align*}
    \rho\frac{D u}{Dt} &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2}  \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 v}{\partial x \partial y} + \frac{\partial^2 w}{\partial x \partial z}\right) + \rho g_x\\
    \rho\frac{D v}{Dt} &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2}  \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial y\partial x} + \frac{\partial^2 v}{\partial y^2}  + \frac{\partial^2 w}{\partial y \partial z}\right) + \rho g_y\\
    \rho\frac{D w}{Dt} &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2}  \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial z \partial x} + \frac{\partial^2 v}{\partial z \partial y} + \frac{\partial^2 w}{\partial z^2}\right) + \rho g_z
\end{align*}
$$

### Incompressible Navier-Stokes equations

Assuming $$\mu$$ is constant, $$\rho$$ is constant, and a Newtonian fluid, we get the incompressible Navier-Stokes equations:

$$\rho \frac{D\vec{V}}{Dt} = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \rho \vec{g}$$


$$\rho \frac{D v_j}{Dt} = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \rho g_j$$

$$\begin{align*}
    \rho\frac{D u}{Dt} &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2}  \right)  + \rho g_x\\
    \rho\frac{D v}{Dt} &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2}  \right) +  \rho g_y\\
    \rho\frac{D w}{Dt} &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2}  \right) + \rho g_z
\end{align*}$$

We can also write the incompressible Navier-Stokes equations in terms of kinematic viscosity $$\nu = \mu/\rho$$:

$$ \frac{D\vec{V}}{Dt} = - \frac{1}{\nu}\vec{\nabla} P + \nu \nabla^2 \vec{V} + \vec{g}$$


$$ \frac{D v_j}{Dt} = - \frac{1}{\nu}\frac{\partial P}{\partial x_j} + \nu \frac{\partial^2v_j}{\partial x_i \partial x_i} + g_j$$

$$\begin{align*}
    \frac{D u}{Dt} &= - \frac{1}{\nu}\frac{\partial P}{\partial x} + \nu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2}  \right)  +  g_x\\
    \frac{D v}{Dt} &= - \frac{1}{\nu}\frac{\partial P}{\partial y} + \nu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2}  \right) +   g_y\\
    \frac{D w}{Dt} &= - \frac{1}{\nu}\frac{\partial P}{\partial z} + \nu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2}  \right) +  g_z
\end{align*}$$


If you find any errors in the above, please contact me (rmcconke@uwaterloo.ca) so that I can fix them. If you found this useful, please let me know!
