---
layout: post
title: The Navier-Stokes Rosetta Stone
date: 2023-12-13 18:00:00-0500
description: Several different forms of the Navier-Stokes equations
tags: fluids education math theory
toc:
  sidebar: left
background: /images/rock.jpg # Add this line
---

## Introduction
The Rosetta stone is an important rock that helped us [understand Egyptian hieroglyphics](https://en.wikipedia.org/wiki/Rosetta_Stone). I know that I certainly find it easier to understand a notation when it's written beside a notation I already know. And I also know that sometimes, understanding an equation is as hard as understanding Egyptian hieroglyphics.

In this post, I will give several important equations in fluid mechanics in three forms: vector calculus notation, Einstein notation, and with the Cartesian components written out. I start with the more general Cauchy equation and explain when we make certain assumptions to get the compressible and incompressible Navier-Stokes equations.

The general roadmap is:

1. Define: tensors, operations, material derivative, stress tensor, viscous stress tensor.
2. Continuity equation: compressible, then incompressible ($\rho$ is constant).
3. Momentum equation: 
    1. Cauchy's momentum equation for a general continuum,
    2. Expand it in terms of isotropic/deviatoric stresses.
    3. Apply Newtonian fluid to get compressible Navier-Stokes.
    4. Apply constant density to 3. to get incompressible Navier-Stokes.  

References I used to compile this post:
1. My favourite fluid mechanics book: &Ccedil;engel and Cimbala, *Fluid Mechanics: Fundamentals and Applications*, McGraw Hill, 4th Edition 
2. Pope, *Turbulent Flows*, Cambridge University Press.
3. [Wikipedia Page](https://en.wikipedia.org/wiki/Navier%E2%80%93Stokes_equations) on Navier-Stokes Equations

## Prerequisites

### Tensors

| Notation | Vector (rank 1 tensor) | Velocity vector | Matrix (rank 2 tensor) | Identity matrix |
|:---:|:---:|:---:|:---:|:---:|
| Vector calculus notation | $\vec{a}$ | $\vec{V}$ | $A$ | $I$ |
| Einstein notation | $a_i$ | $v_i$ | $A_{ij}$ | $\delta_{ij}$ |
| Cartesian components | $\begin{bmatrix} a_x \\\\ a_y \\\\ a_z \end{bmatrix}$ | $\begin{bmatrix} u \\\\ v \\\\ w \end{bmatrix}$ | $\begin{bmatrix} A_{xx} & A_{xy} & A_{xz} \\\\ A_{yx} & A_{yy} & A_{yz} \\\\ A_{zx} & A_{zy} & A_{zz} \end{bmatrix}$ | $\begin{bmatrix} 1 & 0 & 0 \\\\ 0 & 1 & 0 \\\\ 0 & 0 & 1 \end{bmatrix}$ |

### Operations (Part 1)

| Notation | Dot product | Divergence of a vector | Vector-matrix product |
|:---:|:---:|:---:|:---:|
| Vector calculus notation | $\vec{a}\cdot \vec{b}$ | $\vec{\nabla}\cdot \vec{a}$ | $\vec{a}\cdot B$ |
| Einstein notation | $a_i b_i$ | $\frac{\partial a_i}{\partial x_i}$ | $a_i B_{ij}$ |
| Cartesian components | $a_x b_x + a_y b_y + a_z b_z$ | $\frac{\partial a_x}{\partial x} + \frac{\partial a_y}{\partial y} + \frac{\partial a_z}{\partial z}$ | $\begin{bmatrix} a_x B_{xx} & a_yB_{xy} & a_zB_{xz} \\\\ a_x B_{yx} & a_yB_{yy} & a_zB_{yz} \\\\ a_x B_{zx} & a_yB_{zy} & a_zB_{zz} \end{bmatrix}$ |

### Operations (Part 2)

| Notation | Divergence of a tensor | Tensor product or outer product | Gradient of a vector |
|:---:|:---:|:---:|:---:|
| Vector calculus notation | $\vec{\nabla}\cdot A$ | $\vec{a}\vec{b} \equiv \vec{a}\otimes\vec{b}$ | $\vec{\nabla} \vec{a}$ |
| Einstein notation | $\frac{\partial A_{ij}}{\partial x_i}$ | $a_i b_j$ | $\frac{\partial a_i}{\partial x_j}$ |
| Cartesian components | $\begin{bmatrix} \frac{\partial B_{xx}}{\partial x} + \frac{\partial B_{yx}}{\partial y} + \frac{\partial B_{zx}}{\partial z} \\\\ \frac{\partial B_{xy}}{\partial x} + \frac{\partial B_{yy}}{\partial y} + \frac{\partial B_{zy}}{\partial z} \\\\ \frac{\partial B_{xz}}{\partial x} + \frac{\partial B_{yz}}{\partial y} + \frac{\partial B_{zz}}{\partial z} \end{bmatrix}$ | $\begin{bmatrix} a_x b_x & a_x b_y & a_x b_z \\\\ a_y b_x & a_y b_y & a_y b_z \\\\ a_z b_x & a_z b_y & a_z b_z \end{bmatrix}$ | $\begin{bmatrix} \frac{\partial a_x}{\partial x} & \frac{\partial a_x}{\partial y} & \frac{\partial a_x}{\partial z} \\\\ \frac{\partial a_y}{\partial x} & \frac{\partial a_y}{\partial y} & \frac{\partial a_y}{\partial z} \\\\ \frac{\partial a_z}{\partial x} & \frac{\partial a_z}{\partial y} & \frac{\partial a_z}{\partial z} \end{bmatrix}$ |

### Material derivative of velocity

| Notation | Material derivative of velocity |
|:---:|:---:|
| Vector calculus notation | $\frac{D\vec{V}}{Dt}=\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}$ |
| Einstein notation | $\frac{Dv_j}{Dt}=\frac{\partial v_j}{\partial t}+v_i \frac{\partial v_j}{\partial x_i}$ |
| Cartesian components | $\begin{aligned}\frac{Du}{Dt} &= \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} \\\\ \frac{Dv}{Dt} &= \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} \\\\ \frac{Dw}{Dt} &= \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\end{aligned}$ |

### Stress tensor

| Notation | Stress tensor |
|:---:|:---:|
| Vector calculus notation | $\sigma = -P I + \tau$ |
| Einstein notation | $\sigma_{ij}= -P\delta_{ij} + \tau_{ij}$ |
| Cartesian components | $\sigma = -\begin{bmatrix}P & 0 & 0 \\\\ 0 & P & 0 \\\\ 0 & 0 & P \end{bmatrix} + \begin{bmatrix}\tau_{xx} & \tau_{xy} & \tau_{xz} \\\\ \tau_{yx} & \tau_{yy} & \tau_{yz} \\\\ \tau_{zx} & \tau_{zy} & \tau_{zz}\end{bmatrix}$ |

### Viscous stress tensor

| Notation | Viscous stress tensor |
|:---:|:---:|
| Vector calculus notation | $\tau = \zeta (\vec{\nabla}\cdot \vec{V})I + \mu [\vec{\nabla}\vec{V} + (\vec{\nabla}\vec{V})^\text{T} - \tfrac{2}{3}(\vec{\nabla} \cdot \vec{V})I ]$ |
| Einstein notation | $\tau_{ij} = \zeta \frac{\partial v_k}{\partial x_k}\delta_{ij} + \mu \left[\frac{\partial v_j}{\partial x_i} + \frac{\partial v_i}{\partial x_j} - \frac{2}{3}\frac{\partial v_k}{\partial x_k} \delta_{ij}\right]$ |
| Cartesian components | $\begin{aligned} \tau = &\zeta \begin{bmatrix} \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 & 0 \\\\ 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 \\\\ 0 & 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \\\\ &+ \mu \begin{bmatrix} 2\frac{\partial u}{\partial x} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial v}{\partial x} + \frac{\partial u}{\partial y} & \frac{\partial w}{\partial x} + \frac{\partial u}{\partial z} \\\\ \frac{\partial u}{\partial y} + \frac{\partial v}{\partial x} & 2\frac{\partial v}{\partial y} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial w}{\partial y} + \frac{\partial v}{\partial z} \\\\ \frac{\partial u}{\partial z} + \frac{\partial w}{\partial x} & \frac{\partial v}{\partial z} + \frac{\partial w}{\partial y} & 2\frac{\partial w}{\partial z} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \end{aligned}$ |

### Compressible continuity equation

| Notation | Compressible continuity equation |
|:---:|:---:|
| Vector calculus notation | $\frac{\partial \rho}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V}) = 0$ |
| Einstein notation | $\frac{\partial \rho}{\partial t} + \frac{\partial (\rho v_j)}{\partial x_j} = 0$ |
| Cartesian components | $\frac{\partial \rho}{\partial t} + \frac{\partial(\rho u)}{\partial x} + \frac{\partial (\rho v)}{\partial y} + \frac{\partial (\rho w)}{\partial z} = 0$ |

### Incompressible continuity equation

| Notation | Incompressible continuity equation |
|:---:|:---:|
| Vector calculus notation | $\vec{\nabla} \cdot \vec{V} = 0$ |
| Einstein notation | $\frac{\partial v_j}{\partial x_j} = 0$ |
| Cartesian components | $\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z} = 0$ |

### Cauchy's equation

| Notation | Cauchy's equation |
|:---:|:---:|
| Vector calculus notation | $\frac{\partial (\rho \vec{V})}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V} \vec{V}) = \rho \vec{g} + \vec{\nabla} \cdot \sigma$ |
| Einstein notation | $\frac{\partial (\rho v_j)}{\partial t} + \frac{\partial }{\partial x_i}(\rho v_j v_i) = \rho g_j + \frac{\partial \sigma_{ij}}{\partial x_i}$ |
| Cartesian components | $\begin{aligned} \frac{\partial(\rho u)}{\partial t} + \frac{\partial(\rho uu)}{\partial x} + \frac{\partial (\rho uv)}{\partial y} + \frac{\partial (\rho uw)}{\partial z} &= \rho g_x + \frac{\partial \sigma_{xx}}{\partial x} + \frac{\sigma_{yx}}{\partial y} + \frac{\sigma_{zx}}{\partial z} \\\\ \frac{\partial (\rho v)}{\partial t } + \frac{\partial (\rho u v)}{\partial x } + \frac{\partial (\rho v v)}{\partial y} + \frac{\partial (\rho v w)}{\partial z} &= \rho g_y + \frac{\partial \sigma_{xy}}{\partial x} + \frac{\sigma_{yy}}{\partial y} + \frac{\sigma_{zy}}{\partial z} \\\\ \frac{\partial (\rho w)}{\partial t} + \frac{\partial (\rho u w)}{\partial x} + \frac{\partial(\rho v w)}{\partial y} + \frac{\partial (\rho w w)}{\partial z} &= \rho g_z + \frac{\partial \sigma_{xz}}{\partial x} + \frac{\sigma_{yz}}{\partial y} + \frac{\sigma_{zz}}{\partial z} \end{aligned}$ |

### Cauchy's equation - expanded

| Notation | Cauchy's equation - expanded |
|:---:|:---:|
| Vector calculus notation | $\rho(\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \vec{\nabla} \cdot \tau + \rho \vec{g}$ |
| Einstein notation | $\rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = -\frac{\partial P}{\partial x_j} + \frac{\partial \tau_{ij}}{\partial x_i} + \rho g_j$ |
| Cartesian components | $\begin{aligned} \rho (\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z})&= - \frac{\partial P}{\partial x} + \frac{\partial \tau_{xx}}{\partial x}+ \frac{\partial \tau_{yx}}{\partial y}+ \frac{\partial \tau_{zx}}{\partial z} + \rho g_x \\\\ \rho (\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z})&= - \frac{\partial P}{\partial y} + \frac{\partial \tau_{xy}}{\partial x}+ \frac{\partial \tau_{yy}}{\partial y}+ \frac{\partial \tau_{zy}}{\partial z} + \rho g_y \\\\ \rho (\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z})&= - \frac{\partial P}{\partial z} + \frac{\partial \tau_{xz}}{\partial x}+ \frac{\partial \tau_{yz}}{\partial y}+ \frac{\partial \tau_{zz}}{\partial z} + \rho g_z \end{aligned}$ |

### Compressible Navier-Stokes equations
Assuming $\mu$ is constant, and a Newtonian fluid, we get the compressible Navier-Stokes equations:

| Notation | Compressible Navier-Stokes equations |
|:---:|:---:|
| Vector calculus notation | $\rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \tfrac{1}{3}\mu \vec{\nabla}(\vec{\nabla}\cdot\vec{V}) + \rho \vec{g}$ |
| Einstein notation | $\rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \frac{1}{3}\mu \frac{\partial^2 v_i}{\partial x_j \partial x_i} + \rho g_j$ |
| Cartesian components | $\begin{aligned} \rho(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}) &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 v}{\partial x \partial y} + \frac{\partial^2 w}{\partial x \partial z}\right) + \rho g_x \\\\ \rho(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}) &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial y\partial x} + \frac{\partial^2 v}{\partial y^2} + \frac{\partial^2 w}{\partial y \partial z}\right) + \rho g_y \\\\ \rho(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}) &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial z \partial x} + \frac{\partial^2 v}{\partial z \partial y} + \frac{\partial^2 w}{\partial z^2}\right) + \rho g_z \end{aligned}$ |

### Incompressible Navier-Stokes equations
Assuming $\mu$ is constant, $\rho$ is constant, and a Newtonian fluid, we get the incompressible Navier-Stokes equations:

| Notation | Incompressible Navier-Stokes equations |
|:---:|:---:|
| Vector calculus notation | $\rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \rho \vec{g}$ |
| Einstein notation | $\rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \rho g_j$ |
| Cartesian components | $\begin{aligned} \rho{\left(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}\right)} &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \rho g_x \\\\ \rho{\left(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}\right)} &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \rho g_y \\\\ \rho{\left(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\right)} &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \rho g_z \end{aligned}$ |

We can also write the incompressible Navier-Stokes equations in terms of kinematic viscosity $\nu = \mu/\rho$:

| Notation | Incompressible Navier-Stokes equations with kinematic viscosity |
|:---:|:---:|
| Vector calculus notation | $\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V} = - \frac{1}{\rho}\vec{\nabla} P + \nu \nabla^2 \vec{V} + \vec{g}$ |
| Einstein notation | $\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i} = - \frac{1}{\rho}\frac{\partial P}{\partial x_j} + \nu \frac{\partial^2v_j}{\partial x_i \partial x_i} + g_j$ |
| Cartesian components | $\begin{aligned} \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial x} + \nu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + g_x \\\\ \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial y} + \nu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + g_y \\\\ \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial z} + \nu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + g_z \end{aligned}$ |