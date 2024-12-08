---
layout: post
title: The Navier-Stokes Rosetta Stone
date: 2023-12-13 18:00:00-0500
description: Several different forms of the Navier-Stokes equations
tags: fluids education math theory
toc:
  sidebar: left
background: '#333333'  # Dark grey color
color: white
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


We typically work with tensors up to rank 2 in fluids. A rank 0 tensor is a scalar. A rank 1 tensor is a vector. A rank 2 tensor can be visualized a matrix. A [tensor](https://www.youtube.com/watch?v=aibZkcOsmcE) is a geometric object - the fact that something is a tensor allows us to define how it transforms. It's a physical object. For example, a scalar is independent of the coordinate system - you can't rotate a scalar. But you can rotate a vector or rank 2 tensor.
<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Tensors</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Vector (rank 1 tensor)</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Velocity vector</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Matrix (rank 2 tensor)</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Identity matrix</th>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{a}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{V}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle A$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle I$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle a_i$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle v_i$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle A_{ij}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \delta_{ij}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} a_x \\ a_y \\ a_z \end{bmatrix}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} u \\ v \\ w \end{bmatrix}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} A_{xx} & A_{xy} & A_{xz} \\ A_{yx} & A_{yy} & A_{yz} \\ A_{zx} & A_{zy} & A_{zz} \end{bmatrix}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>

Some operations between tensors are defined, whereas other are not. While the tensors might be the words and letters of fluid mechanics, the operations are the sentence structure, grammar, spelling, and punctuation. An important set of operations involves a tensor and the $\nabla$ operator. If we take the *divergence* of a tensor, we *reduce its rank by 1*. If we take the gradient of a tensor, we *increase its rank by 1*. For example, the gradient of a scalar is a vector - its a vector who's magnitude indicates the rate of change, and the components of the vector indicates the amount of change along each coordinate axis. The *divergence* of rank 0 tensor (scalar) is not defined. The rank of a scalar can't be reduced anymore - we can't have a rank -1 tensor.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Operations</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Dot product</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Divergence of a vector</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Vector-matrix product</th>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{a}\cdot \vec{b}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{\nabla}\cdot \vec{a}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{a}\cdot B$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle a_i b_i$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial a_i}{\partial x_i}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle a_i B_{ij}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle a_x b_x + a_y b_y + a_z b_z$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \frac{\partial a_x}{\partial x} + \frac{\partial a_y}{\partial y} + \frac{\partial a_z}{\partial z}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} a_x B_{xx} & a_yB_{xy} & a_zB_{xz} \\ a_x B_{yx} & a_yB_{yy} & a_zB_{yz} \\ a_x B_{zx} & a_yB_{zy} & a_zB_{zz} \end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">More operations</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Divergence of a rank 2 tensor</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Tensor product or outer product</th>
<th style="text-align: center; border-bottom: 1px solid white; padding: 10px;">Gradient of a vector</th>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{\nabla}\cdot A$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{a}\vec{b} \equiv \vec{a}\otimes\vec{b}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{\nabla} \vec{a}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial A_{ij}}{\partial x_i}$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle a_i b_j$</td>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial a_i}{\partial x_j}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} \frac{\partial B_{xx}}{\partial x} + \frac{\partial B_{yx}}{\partial y} + \frac{\partial B_{zx}}{\partial z} \\ \frac{\partial B_{xy}}{\partial x} + \frac{\partial B_{yy}}{\partial y} + \frac{\partial B_{zy}}{\partial z} \\ \frac{\partial B_{xz}}{\partial x} + \frac{\partial B_{yz}}{\partial y} + \frac{\partial B_{zz}}{\partial z} \end{bmatrix}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} a_x b_x & a_x b_y & a_x b_z \\ a_y b_x & a_y b_y & a_y b_z \\ a_z b_x & a_z b_y & a_z b_z \end{bmatrix}$</td>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{bmatrix} \frac{\partial a_x}{\partial x} & \frac{\partial a_x}{\partial y} & \frac{\partial a_x}{\partial z} \\ \frac{\partial a_y}{\partial x} & \frac{\partial a_y}{\partial y} & \frac{\partial a_y}{\partial z} \\ \frac{\partial a_z}{\partial x} & \frac{\partial a_z}{\partial y} & \frac{\partial a_z}{\partial z} \end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>





The material/total/substantial derivative is an important quantity in continuum mechanics. It is denoted with a capital $D$ (the usual derivative is a lowercase $d$). It is the rate of change of a quantity per unit time in a frame of reference that moves with the fluid (Lagrangian perspective). It says that when we follow a chunk of fluid (i.e. Lagrangian perspective) ,the total time rate of change equals the local time rate of change of the Eulerian field, plus the spatial rate of change multiplied by how fast we're moving through space. For more details about the material derivative, see my [lecture on the material derivative](https://www.youtube.com/watch?v=g_7_hP3yX_8).
<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Material derivative</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{D\vec{V}}{Dt}=\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{Dv_j}{Dt}=\frac{\partial v_j}{\partial t}+v_i \frac{\partial v_j}{\partial x_i}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned}\frac{Du}{Dt} &= \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} \\ \frac{Dv}{Dt} &= \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} \\ \frac{Dw}{Dt} &= \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

A *stress tensor* is a rank 2 tensor that gives us the individual stresses on each face of a fluid element. It's convenient to separate this tensor into *pressure* (isotropic) and *viscous* (eventually, this will be deviatoric) components.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Stress Tensor</span>, decomposed into isotropic and deviatoric parts
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \sigma = -P I + \tau$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \sigma_{ij}= -P\delta_{ij} + \tau_{ij}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \sigma = -\begin{bmatrix}P & 0 & 0 \\ 0 & P & 0 \\ 0 & 0 & P \end{bmatrix} + \begin{bmatrix}\tau_{xx} & \tau_{xy} & \tau_{xz} \\ \tau_{yx} & \tau_{yy} & \tau_{yz} \\ \tau_{zx} & \tau_{zy} & \tau_{zz}\end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>


<hr class="remark-start">
<div style="text-align: right;">
<span class="ballet-fancy">Remark</span>

*Any* rank 2 tensor (for example, the Cauchy stress tensor) can be decomposed into an *isotropic* tensor and a *deviatoric* tensor:

$$ A_{ij} = A^{iso}_{ij} + A^{dev}_{ij} = \frac{1}{3}A_{kk}\delta_{ij} + \left(A_{ij} - \frac{1}{3}A_{kk}\delta_{ij}\right) $$

The isotropic tensor is the same in all directions. It is a scalar multiple of the identity tensor. The remaining part of the tensor (after we subtract away the isotropic part) is the deviatoric part.
</div>
<hr class="remark-end">

The *viscous stress tensor* is a rank 2 tensor that gives us the individual viscous stresses on each face of a fluid element. Viscous stresses are created due to velocity gradients - if a certain part of the fluid is moving faster than another part, then a viscous stress which slows the faster part and speeds up the slower part will be created. *Viscosity* is a fluid property that tells us how much viscous stress is created by a velocity gradient. In fact, there are two types of viscosity - *bulk viscosity* $\zeta$ and *dynamic viscosity* $\mu$. Bulk viscosity is related to the rate of change of volume: notice that the diagonals give the volumetric strain rate ([lecture here](https://www.youtube.com/watch?v=zHAilIvxuiU&list=PLuV-XJJZrRRdR_fZkK2JFPJcnh6oagg20&index=15)). 

There are several reasons that we typically only consider shear viscosity in fluids. First, bulk viscosity is only important in highly compressible fluids, because for an incompressible fluid, conservation of mass means that the volumetric strain rate is zero (see again my [lecture on this](https://www.youtube.com/watch?v=zHAilIvxuiU&list=PLuV-XJJZrRRdR_fZkK2JFPJcnh6oagg20&index=15)). Second, for many fluids, the bulk viscosity is negligible. For this reason, we typically only consider the shear stress, making the viscous stress tensor $\tau$ *deviatoric*.

<div class="remark-environment">
    <span class="ballet-fancy">Remark.</span>
    <p>The part of the viscous stress tensor related to bulk viscosity is an isotropic tensor.</p>
</div>

Here, I give the full viscous stress tensor. For most fluids analysis, the bulk viscosity is negligible, so we typically only consider the second (deviatoric) part.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">The viscous stress tensor</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \tau = \zeta (\vec{\nabla}\cdot \vec{V})I + \mu [\vec{\nabla}\vec{V} + (\vec{\nabla}\vec{V})^\text{T} - \tfrac{2}{3}(\vec{\nabla} \cdot \vec{V})I ]$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \tau_{ij} = \zeta \frac{\partial v_k}{\partial x_k}\delta_{ij} + \mu \left[\frac{\partial v_j}{\partial x_i} + \frac{\partial v_i}{\partial x_j} - \frac{2}{3}\frac{\partial v_k}{\partial x_k} \delta_{ij}\right]$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \tau = &\zeta \begin{bmatrix} \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 & 0 \\ 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 \\ 0 & 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \\ &+ \mu \begin{bmatrix} 2\frac{\partial u}{\partial x} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial v}{\partial x} + \frac{\partial u}{\partial y} & \frac{\partial w}{\partial x} + \frac{\partial u}{\partial z} \\ \frac{\partial u}{\partial y} + \frac{\partial v}{\partial x} & 2\frac{\partial v}{\partial y} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial w}{\partial y} + \frac{\partial v}{\partial z} \\ \frac{\partial u}{\partial z} + \frac{\partial w}{\partial x} & \frac{\partial v}{\partial z} + \frac{\partial w}{\partial y} & 2\frac{\partial w}{\partial z} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

<hr class="remark-start">
<div style="text-align: right;">
<span class="ballet-fancy">Remark</span>

For most fluids, the viscous stress tensor is deviatoric.
</div>
<hr class="remark-end">


## Conservation of mass - the continuity equation

The compressible continuity equation is the more general case - we allow the density $\rho$ to change.
<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Compressible continuity equation</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial \rho}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V}) = 0$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial \rho}{\partial t} + \frac{\partial (\rho v_j)}{\partial x_j} = 0$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \frac{\partial \rho}{\partial t} + \frac{\partial(\rho u)}{\partial x} + \frac{\partial (\rho v)}{\partial y} + \frac{\partial (\rho w)}{\partial z} = 0$</td>
</tr>
</tbody>
</table>
</div>

If we take density $\rho$ to be constant (physically, we can't actually compress the fluid), then we get the incompressible continuity equation.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Incompressible continuity equation</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \vec{\nabla} \cdot \vec{V} = 0$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial v_j}{\partial x_j} = 0$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z} = 0$</td>
</tr>
</tbody>
</table>
</div>

## Conservation of momentum

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Cauchy's equation</span>
Conservation of momentum for a general continuum
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial (\rho \vec{V})}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V} \vec{V}) = \rho \vec{g} + \vec{\nabla} \cdot \sigma$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial (\rho v_j)}{\partial t} + \frac{\partial }{\partial x_i}(\rho v_j v_i) = \rho g_j + \frac{\partial \sigma_{ij}}{\partial x_i}$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \frac{\partial(\rho u)}{\partial t} + \frac{\partial(\rho uu)}{\partial x} + \frac{\partial (\rho uv)}{\partial y} + \frac{\partial (\rho uw)}{\partial z} &= \rho g_x + \frac{\partial \sigma_{xx}}{\partial x} + \frac{\sigma_{yx}}{\partial y} + \frac{\sigma_{zx}}{\partial z} \\ \frac{\partial (\rho v)}{\partial t } + \frac{\partial (\rho u v)}{\partial x } + \frac{\partial (\rho v v)}{\partial y} + \frac{\partial (\rho v w)}{\partial z} &= \rho g_y + \frac{\partial \sigma_{xy}}{\partial x} + \frac{\sigma_{yy}}{\partial y} + \frac{\sigma_{zy}}{\partial z} \\ \frac{\partial (\rho w)}{\partial t} + \frac{\partial (\rho u w)}{\partial x} + \frac{\partial(\rho v w)}{\partial y} + \frac{\partial (\rho w w)}{\partial z} &= \rho g_z + \frac{\partial \sigma_{xz}}{\partial x} + \frac{\sigma_{yz}}{\partial y} + \frac{\sigma_{zz}}{\partial z} \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Cauchy's equation</span> with pressure (isotropic) and viscous (deviatoric) stress terms shown explicitly
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho(\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \vec{\nabla} \cdot \tau + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = -\frac{\partial P}{\partial x_j} + \frac{\partial \tau_{ij}}{\partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \rho (\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z})&= - \frac{\partial P}{\partial x} + \frac{\partial \tau_{xx}}{\partial x}+ \frac{\partial \tau_{yx}}{\partial y}+ \frac{\partial \tau_{zx}}{\partial z} + \rho g_x \\\\ \rho (\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z})&= - \frac{\partial P}{\partial y} + \frac{\partial \tau_{xy}}{\partial x}+ \frac{\partial \tau_{yy}}{\partial y}+ \frac{\partial \tau_{zy}}{\partial z} + \rho g_y \\\\ \rho (\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z})&= - \frac{\partial P}{\partial z} + \frac{\partial \tau_{xz}}{\partial x}+ \frac{\partial \tau_{yz}}{\partial y}+ \frac{\partial \tau_{zz}}{\partial z} + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

### Compressible Navier-Stokes equations
If we substitute our stress tensor into Cauchy's equation, we get the compressible Navier-Stokes equations.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Compressible Navier-Stokes Equations</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \tfrac{1}{3}\mu \vec{\nabla}(\vec{\nabla}\cdot\vec{V}) + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \frac{1}{3}\mu \frac{\partial^2 v_i}{\partial x_j \partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \rho(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}) &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 v}{\partial x \partial y} + \frac{\partial^2 w}{\partial x \partial z}\right) + \rho g_x \\\\ \rho(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}) &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial y\partial x} + \frac{\partial^2 v}{\partial y^2} + \frac{\partial^2 w}{\partial y \partial z}\right) + \rho g_y \\\\ \rho(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}) &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial z \partial x} + \frac{\partial^2 v}{\partial z \partial y} + \frac{\partial^2 w}{\partial z^2}\right) + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

### Incompressible Navier-Stokes equations
If we again take density $\rho$ to be constant, then we get the incompressible Navier-Stokes equations.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Incompressible Navier-Stokes Equations</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \rho{\left(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}\right)} &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \rho g_x \\\\ \rho{\left(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}\right)} &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \rho g_y \\\\ \rho{\left(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\right)} &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

The incompressible Navier-Stokes equations can be written in terms of the kinematic viscosity $\nu = \frac{\mu}{\rho}$. If we divide the Navier-Stokes equations by $\rho$, we get the following.

<div style="display: flex; align-items: center; margin-bottom: 40px;">
<div style="flex: 0 0 200px; text-align: center;">
<span class="parisienne-regular">Incompressible Navier-Stokes Equations (kinematic units)</span>
</div>
<table style="flex: 1; border-collapse: collapse;">
<tbody>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V} = - \frac{1}{\rho}\vec{\nabla} P + \nu \nabla^2 \vec{V} + \vec{g}$</td>
</tr>
<tr>
<td style="text-align: center; border-bottom: 1px solid white; padding: 10px;">$\displaystyle \frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i} = - \frac{1}{\rho}\frac{\partial P}{\partial x_j} + \nu \frac{\partial^2v_j}{\partial x_i \partial x_i} + g_j$</td>
</tr>
<tr>
<td style="text-align: center; padding: 10px;">$\displaystyle \begin{aligned} \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial x} + \nu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + g_x \\\\ \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial y} + \nu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + g_y \\\\ \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial z} + \nu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + g_z \end{aligned}$</td>
</tr>
</tbody>
</table>
</div>

Often, we move $1/\rho$ into the pressure gradient term, and define a new pressure $p \equiv P/\rho$. This new pressure is sometimes called the *kinematic pressure* (it's just a scalar multiple of the mechanical pressure). Therefore, the kinematic flavour of the Navier-Stokes equations describe the relationship between velocity and kinematic pressure, with a single fluid property $\nu$ (the kinematic viscosity).