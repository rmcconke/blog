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

<div style="display: flex; align-items: center;">
<div style="flex: 0 0 200px; text-align: center;">
Basic tensor notation showing vectors, velocity, matrices, and the identity matrix.
</div>
<table style="flex: 1;">
<thead>
<tr>
<th style="text-align: center">Vector (rank 1 tensor)</th>
<th style="text-align: center">Velocity vector</th>
<th style="text-align: center">Matrix (rank 2 tensor)</th>
<th style="text-align: center">Identity matrix</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">$\displaystyle \vec{a}$</td>
<td style="text-align: center">$\displaystyle \vec{V}$</td>
<td style="text-align: center">$\displaystyle A$</td>
<td style="text-align: center">$\displaystyle I$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: center">$\displaystyle a_i$</td>
<td style="text-align: center">$\displaystyle v_i$</td>
<td style="text-align: center">$\displaystyle A_{ij}$</td>
<td style="text-align: center">$\displaystyle \delta_{ij}$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: center">$\displaystyle \begin{bmatrix} a_x \\ a_y \\ a_z \end{bmatrix}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} u \\ v \\ w \end{bmatrix}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} A_{xx} & A_{xy} & A_{xz} \\ A_{yx} & A_{yy} & A_{yz} \\ A_{zx} & A_{zy} & A_{zz} \end{bmatrix}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>

### Basic Operations

<div style="display: flex; align-items: center;">
<div style="flex: 0 0 200px;">
Common vector operations including dot product, divergence, and vector-matrix multiplication.
</div>
<table style="flex: 1;">
<thead>
<tr>
<th style="text-align: center">Dot product</th>
<th style="text-align: center">Divergence of a vector</th>
<th style="text-align: center">Vector-matrix product</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">$\displaystyle \vec{a}\cdot \vec{b}$</td>
<td style="text-align: center">$\displaystyle \vec{\nabla}\cdot \vec{a}$</td>
<td style="text-align: center">$\displaystyle \vec{a}\cdot B$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: center">$\displaystyle a_i b_i$</td>
<td style="text-align: center">$\displaystyle \frac{\partial a_i}{\partial x_i}$</td>
<td style="text-align: center">$\displaystyle a_i B_{ij}$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: center">$\displaystyle a_x b_x + a_y b_y + a_z b_z$</td>
<td style="text-align: center">$\displaystyle \frac{\partial a_x}{\partial x} + \frac{\partial a_y}{\partial y} + \frac{\partial a_z}{\partial z}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} a_x B_{xx} & a_yB_{xy} & a_zB_{xz} \\ a_x B_{yx} & a_yB_{yy} & a_zB_{yz} \\ a_x B_{zx} & a_yB_{zy} & a_zB_{zz} \end{bmatrix}$</td>
</tr>
</tbody>
</table>
</div>

### Operations (Part 2)

<table>
<thead>
<tr>
<th style="text-align: center">Notation</th>
<th style="text-align: center">Divergence of a tensor</th>
<th style="text-align: center">Tensor product or outer product</th>
<th style="text-align: center">Gradient of a vector</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">Vector calculus notation</td>
<td style="text-align: center">$\displaystyle \vec{\nabla}\cdot A$</td>
<td style="text-align: center">$\displaystyle \vec{a}\vec{b} \equiv \vec{a}\otimes\vec{b}$</td>
<td style="text-align: center">$\displaystyle \vec{\nabla} \vec{a}$</td>
</tr>
<tr>
<td style="text-align: center">Einstein notation</td>
<td style="text-align: center">$\displaystyle \frac{\partial A_{ij}}{\partial x_i}$</td>
<td style="text-align: center">$\displaystyle a_i b_j$</td>
<td style="text-align: center">$\displaystyle \frac{\partial a_i}{\partial x_j}$</td>
</tr>
<tr>
<td style="text-align: center">Cartesian components</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} \frac{\partial B_{xx}}{\partial x} + \frac{\partial B_{yx}}{\partial y} + \frac{\partial B_{zx}}{\partial z} \\\\ \frac{\partial B_{xy}}{\partial x} + \frac{\partial B_{yy}}{\partial y} + \frac{\partial B_{zy}}{\partial z} \\\\ \frac{\partial B_{xz}}{\partial x} + \frac{\partial B_{yz}}{\partial y} + \frac{\partial B_{zz}}{\partial z} \end{bmatrix}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} a_x b_x & a_x b_y & a_x b_z \\\\ a_y b_x & a_y b_y & a_y b_z \\\\ a_z b_x & a_z b_y & a_z b_z \end{bmatrix}$</td>
<td style="text-align: center">$\displaystyle \begin{bmatrix} \frac{\partial a_x}{\partial x} & \frac{\partial a_x}{\partial y} & \frac{\partial a_x}{\partial z} \\\\ \frac{\partial a_y}{\partial x} & \frac{\partial a_y}{\partial y} & \frac{\partial a_y}{\partial z} \\\\ \frac{\partial a_z}{\partial x} & \frac{\partial a_z}{\partial y} & \frac{\partial a_z}{\partial z} \end{bmatrix}$</td>
</tr>
</tbody>
</table>

### Material derivative of velocity

<table>
<thead>
<tr>
<th style="text-align: center">Notation</th>
<th style="text-align: center">Material derivative of velocity</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">Vector calculus notation</td>
<td style="text-align: center">$\displaystyle \frac{D\vec{V}}{Dt}=\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}$</td>
</tr>
<tr>
<td style="text-align: center">Einstein notation</td>
<td style="text-align: center">$\displaystyle \frac{Dv_j}{Dt}=\frac{\partial v_j}{\partial t}+v_i \frac{\partial v_j}{\partial x_i}$</td>
</tr>
<tr>
<td style="text-align: center">Cartesian components</td>
<td style="text-align: center">$\displaystyle \begin{aligned}\frac{Du}{Dt} &= \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} \\\\ \frac{Dv}{Dt} &= \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} \\\\ \frac{Dw}{Dt} &= \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\end{aligned}$</td>
</tr>
</tbody>
</table>

### Stress tensor

<table>
<thead>
<tr>
<th style="text-align: center">Notation</th>
<th style="text-align: center">Stress tensor</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">Vector calculus notation</td>
<td style="text-align: center">$\displaystyle \sigma = -P I + \tau$</td>
</tr>
<tr>
<td style="text-align: center">Einstein notation</td>
<td style="text-align: center">$\displaystyle \sigma_{ij}= -P\delta_{ij} + \tau_{ij}$</td>
</tr>
<tr>
<td style="text-align: center">Cartesian components</td>
<td style="text-align: center">$\displaystyle \sigma = -\begin{bmatrix}P & 0 & 0 \\\\ 0 & P & 0 \\\\ 0 & 0 & P \end{bmatrix} + \begin{bmatrix}\tau_{xx} & \tau_{xy} & \tau_{xz} \\\\ \tau_{yx} & \tau_{yy} & \tau_{yz} \\\\ \tau_{zx} & \tau_{zy} & \tau_{zz}\end{bmatrix}$</td>
</tr>
</tbody>
</table>

### Viscous stress tensor

<table>
<thead>
<tr>
<th style="text-align: center">Notation</th>
<th style="text-align: center">Viscous stress tensor</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center">Vector calculus notation</td>
<td style="text-align: center">$\displaystyle \tau = \zeta (\vec{\nabla}\cdot \vec{V})I + \mu [\vec{\nabla}\vec{V} + (\vec{\nabla}\vec{V})^\text{T} - \tfrac{2}{3}(\vec{\nabla} \cdot \vec{V})I ]$</td>
</tr>
<tr>
<td style="text-align: center">Einstein notation</td>
<td style="text-align: center">$\displaystyle \tau_{ij} = \zeta \frac{\partial v_k}{\partial x_k}\delta_{ij} + \mu \left[\frac{\partial v_j}{\partial x_i} + \frac{\partial v_i}{\partial x_j} - \frac{2}{3}\frac{\partial v_k}{\partial x_k} \delta_{ij}\right]$</td>
</tr>
<tr>
<td style="text-align: center">Cartesian components</td>
<td style="text-align: center">$\displaystyle \begin{aligned} \tau = &\zeta \begin{bmatrix} \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 & 0 \\\\ 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & 0 \\\\ 0 & 0 & \left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \\\\ &+ \mu \begin{bmatrix} 2\frac{\partial u}{\partial x} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial v}{\partial x} + \frac{\partial u}{\partial y} & \frac{\partial w}{\partial x} + \frac{\partial u}{\partial z} \\\\ \frac{\partial u}{\partial y} + \frac{\partial v}{\partial x} & 2\frac{\partial v}{\partial y} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) & \frac{\partial w}{\partial y} + \frac{\partial v}{\partial z} \\\\ \frac{\partial u}{\partial z} + \frac{\partial w}{\partial x} & \frac{\partial v}{\partial z} + \frac{\partial w}{\partial y} & 2\frac{\partial w}{\partial z} - \frac{2}{3}\left(\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z}\right) \end{bmatrix} \end{aligned}$</td>
</tr>
</tbody>
</table>

### Compressible continuity equation

<table>
<thead>
<tr>
<th style="text-align: left">Compressible continuity equation</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">$\displaystyle \frac{\partial \rho}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V}) = 0$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: left">$\displaystyle \frac{\partial \rho}{\partial t} + \frac{\partial (\rho v_j)}{\partial x_j} = 0$</td>
</tr>
<tr style="border-top: 1px solid black">
<td style="text-align: left">$\displaystyle \frac{\partial \rho}{\partial t} + \frac{\partial(\rho u)}{\partial x} + \frac{\partial (\rho v)}{\partial y} + \frac{\partial (\rho w)}{\partial z} = 0$</td>
</tr>
</tbody>
</table>

### Incompressible continuity equation

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Incompressible continuity equation</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \vec{\nabla} \cdot \vec{V} = 0$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \frac{\partial v_j}{\partial x_j} = 0$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} + \frac{\partial w}{\partial z} = 0$</td>
</tr>
</tbody>
</table>

### Cauchy's equation

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Cauchy's equation</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \frac{\partial (\rho \vec{V})}{\partial t} + \vec{\nabla} \cdot (\rho \vec{V} \vec{V}) = \rho \vec{g} + \vec{\nabla} \cdot \sigma$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \frac{\partial (\rho v_j)}{\partial t} + \frac{\partial }{\partial x_i}(\rho v_j v_i) = \rho g_j + \frac{\partial \sigma_{ij}}{\partial x_i}$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \begin{aligned} \frac{\partial(\rho u)}{\partial t} + \frac{\partial(\rho uu)}{\partial x} + \frac{\partial (\rho uv)}{\partial y} + \frac{\partial (\rho uw)}{\partial z} &= \rho g_x + \frac{\partial \sigma_{xx}}{\partial x} + \frac{\sigma_{yx}}{\partial y} + \frac{\sigma_{zx}}{\partial z} \\\\ \frac{\partial (\rho v)}{\partial t } + \frac{\partial (\rho u v)}{\partial x } + \frac{\partial (\rho v v)}{\partial y} + \frac{\partial (\rho v w)}{\partial z} &= \rho g_y + \frac{\partial \sigma_{xy}}{\partial x} + \frac{\sigma_{yy}}{\partial y} + \frac{\sigma_{zy}}{\partial z} \\\\ \frac{\partial (\rho w)}{\partial t} + \frac{\partial (\rho u w)}{\partial x} + \frac{\partial(\rho v w)}{\partial y} + \frac{\partial (\rho w w)}{\partial z} &= \rho g_z + \frac{\partial \sigma_{xz}}{\partial x} + \frac{\sigma_{yz}}{\partial y} + \frac{\sigma_{zz}}{\partial z} \end{aligned}$</td>
</tr>
</tbody>
</table>

### Cauchy's equation - expanded

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Cauchy's equation - expanded</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \rho(\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \vec{\nabla} \cdot \tau + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = -\frac{\partial P}{\partial x_j} + \frac{\partial \tau_{ij}}{\partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \begin{aligned} \rho (\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z})&= - \frac{\partial P}{\partial x} + \frac{\partial \tau_{xx}}{\partial x}+ \frac{\partial \tau_{yx}}{\partial y}+ \frac{\partial \tau_{zx}}{\partial z} + \rho g_x \\\\ \rho (\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z})&= - \frac{\partial P}{\partial y} + \frac{\partial \tau_{xy}}{\partial x}+ \frac{\partial \tau_{yy}}{\partial y}+ \frac{\partial \tau_{zy}}{\partial z} + \rho g_y \\\\ \rho (\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z})&= - \frac{\partial P}{\partial z} + \frac{\partial \tau_{xz}}{\partial x}+ \frac{\partial \tau_{yz}}{\partial y}+ \frac{\partial \tau_{zz}}{\partial z} + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>

### Compressible Navier-Stokes equations
Assuming $\mu$ is constant, and a Newtonian fluid, we get the compressible Navier-Stokes equations:

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Compressible Navier-Stokes equations</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \tfrac{1}{3}\mu \vec{\nabla}(\vec{\nabla}\cdot\vec{V}) + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \frac{1}{3}\mu \frac{\partial^2 v_i}{\partial x_j \partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \begin{aligned} \rho(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}) &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 v}{\partial x \partial y} + \frac{\partial^2 w}{\partial x \partial z}\right) + \rho g_x \\\\ \rho(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}) &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial y\partial x} + \frac{\partial^2 v}{\partial y^2} + \frac{\partial^2 w}{\partial y \partial z}\right) + \rho g_y \\\\ \rho(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}) &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \frac{1}{3}\mu \left( \frac{\partial^2 u}{\partial z \partial x} + \frac{\partial^2 v}{\partial z \partial y} + \frac{\partial^2 w}{\partial z^2}\right) + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>

### Incompressible Navier-Stokes equations
Assuming $\mu$ is constant, $\rho$ is constant, and a Newtonian fluid, we get the incompressible Navier-Stokes equations:

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Incompressible Navier-Stokes equations</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \rho (\frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V}) = - \vec{\nabla} P + \mu \nabla^2 \vec{V} + \rho \vec{g}$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \rho (\frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i}) = - \frac{\partial P}{\partial x_j} + \mu \frac{\partial^2v_j}{\partial x_i \partial x_i} + \rho g_j$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \begin{aligned} \rho{\left(\frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z}\right)} &= - \frac{\partial P}{\partial x} + \mu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + \rho g_x \\\\ \rho{\left(\frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z}\right)} &= - \frac{\partial P}{\partial y} + \mu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + \rho g_y \\\\ \rho{\left(\frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z}\right)} &= - \frac{\partial P}{\partial z} + \mu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + \rho g_z \end{aligned}$</td>
</tr>
</tbody>
</table>

We can also write the incompressible Navier-Stokes equations in terms of kinematic viscosity $\nu = \mu/\rho$:

<table>
<thead>
<tr>
<th style="text-align: left">Notation</th>
<th style="text-align: left">Incompressible Navier-Stokes equations with kinematic viscosity</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">Vector calculus notation</td>
<td style="text-align: left">$\displaystyle \frac{\partial\vec{V}}{\partial t}+(\vec{V}\cdot \vec{\nabla})\vec{V} = - \frac{1}{\rho}\vec{\nabla} P + \nu \nabla^2 \vec{V} + \vec{g}$</td>
</tr>
<tr>
<td style="text-align: left">Einstein notation</td>
<td style="text-align: left">$\displaystyle \frac{\partial v_j}{\partial t} +v_i \frac{\partial v_j}{\partial x_i} = - \frac{1}{\rho}\frac{\partial P}{\partial x_j} + \nu \frac{\partial^2v_j}{\partial x_i \partial x_i} + g_j$</td>
</tr>
<tr>
<td style="text-align: left">Cartesian components</td>
<td style="text-align: left">$\displaystyle \begin{aligned} \frac{\partial u}{\partial t} + u \frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} + w \frac{\partial u}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial x} + \nu \left( \frac{\partial^2 u}{\partial x^2} +\frac{\partial^2 u}{\partial y^2} +\frac{\partial^2 u}{\partial z^2} \right) + g_x \\\\ \frac{\partial v}{\partial t} + u \frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} + w \frac{\partial v}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial y} + \nu \left( \frac{\partial^2 v}{\partial x^2} +\frac{\partial^2 v}{\partial y^2} +\frac{\partial^2 v}{\partial z^2} \right) + g_y \\\\ \frac{\partial w}{\partial t} + u \frac{\partial w}{\partial x} + v\frac{\partial w}{\partial y} + w \frac{\partial w}{\partial z} &= - \frac{1}{\rho}\frac{\partial P}{\partial z} + \nu \left( \frac{\partial^2 w}{\partial x^2} +\frac{\partial^2 w}{\partial y^2} +\frac{\partial^2 w}{\partial z^2} \right) + g_z \end{aligned}$</td>
</tr>
</tbody>
</table>