---
layout: post
title: The nuts and bolts of the PISO algorithm
date: 2025-04-24 18:00:00-0500
description: A simple explanation of the PISO algorithm
tags: computational-fluid-dynamics education openfoam
toc:
  sidebar: left
background: '#FFFFFF' 
color: black
---

## Introduction

Segregated pressure velocity coupling solvers for the Navier-Stokes equations. Sound familiar? Well, these algorithms were used to design the last car you were in, and the last plane you were in.

For an incompressible, viscous fluid, the Navier-Stokes equations govern the relationship between two variables: pressure $p$, and velocity $\mathbf{U}$. In an incompressible flow, the challenging part of a CFD algorithm is _pressure-velocity coupling_. We don't have an "equation of state" (a separate equation for pressure). All we have are two equations: conservation of mass and conservation of momentum. We simultaneously solve this system of equations. Unfortunately, these equations are quite difficult to solve simultaneously. 


## Equations
The main equation is the _momentum equation_:

$$
\frac{\partial \mathbf{U}}{\partial t} + \nabla \cdot (\mathbf{U} \mathbf{U}) = -\nabla p + \nu \nabla^2 \mathbf{U}
$$
(here, $p$ is the kinematic pressure).


The _continuity equation_ is conservation of mass for an incompressible fluid: $dm/dt = 0$. Since our fluid is incompressible ($\rho$ is constant), this equation is equivalent to a "conservation of volume": $d(\rho V)/dt = 0 \rightarrow dV/dt = 0$. 

$$
\nabla \cdot \mathbf{U} = 0
$$

For more info - check out my blog post on the [Navier-Stokes equations](https://ryleymcconkey.com/2023/12/navier-stokes/).

## Discretization

Let's speedrun through how these two equations are actually solved simultaneously in a CFD algorithm. We _discretize_ these equations on a mesh, using a set of _discretization schemes_ (e.g., upwind, central difference). Often, different terms in the momentum equation are discretized using different schemes. This isn't important - at the end of the day, we end up with a _system of linear equations_.

$$
\frac{\partial \mathbf{U}}{\partial t} + \nabla \cdot (\mathbf{U} \mathbf{U}) = - \nabla p + \nu \nabla^2 \mathbf{U}
$$

Using our _discretization schemes_, this equation is simply written as:

$$
M \mathbf{U} = -\nabla p
$$

The exact schemes used in our discretization of $\mathbf{U}$ are crammed together into one big matrix $M$ of _coefficients_. __These coefficients ($M$) are known. AKA, they are not unknowns. We know $M$. It's a matrix full of numbers.__ Remember: our only two unknowns throughout this are are $p$ and $\mathbf{U}$.

Let's look at our matrix $M$ (remember, it's just matrix full of numbers) for a 3x3 mesh. Let's visualize it using colours. The darker the colour, the larger the number. Let's visualize the discretization matrix $M$ in OpenFOAM. The code to do this is attached to this blog post. An important thing to note is that diagonal dominance of this matrix is very important. Stable algorithms tend to have higher diagonal dominance.



Each row of $M$ corresponds to conservation of momentum for a single cell in the mesh. There are $N$ cells in the mesh, and therefore $M \in \mathbb{R}^{N \times N}$. The diagonal elements are the coefficients for the velocity of the cell itself in the "discretized momentum equation" (i.e., the momentum equation after we substituted in our discretization schemes). The off-diagonal elements are the coefficients for the velocity of all the other cells in the mesh. Conservation of momentum for a given cell usually just considers the neighbouring cells (it's just a momentum balance over a given cell). For example, the momentum balance for the centre cell in this mesh depends on the velocity of the cell itself, and the velocities of the cells to the north, south, east, and west. Can you identify which row in the matrix corresponds to the momentum balance for the centre cell? 

Right, it's the fourth row! It's the only one which depends on four other cells (in this specific 3x3 mesh with non-periodic boundary conditions).

Remark: 
$M$ is:
- symmetric
- mostly zeros

## PISO algorithm

The original reference for the PISO algorithm is from 1986 by [Issa et al.](https://www.sciencedirect.com/science/article/pii/0021999186901002). Despite being almost 40 years old at the time of this post, it's still going strong!

Let's make the following decomposition:

$$
M\mathbf{U} = A\mathbf{U} - H
$$

Where $A$ is the diagonal part of $M$, i.e. this matrix:


Remark 1:
$A^{-1}$ is easily computed. It's just a diagonal matrix with the reciprocal ($1/a_{ij}$) of the diagonal elements of $A$ on the diagonal. i.e., it's this matrix:

Remark 2:
$M \neq A - H$. __The definition of $H$ incorporates $\mathbf{U}$ itself! $A$ is independent of $\mathbf{U}, but $H$ is not.__ A given $H$ comes with a given $\mathbf{U}$.

This decomposition of $M$ leads to two equations:

1. $\mathbf{U} = A^{-1}H - A^{-1}\nabla p$.
2. $\nabla \cdot \left(A^{-1}\nabla p\right) = \nabla \cdot \left(A^{-1}H\right)$.

To get 1., we set $A\mathbf{U} - H = -\nabla p$, then multiply by $A^{-1}$ (remember $A^{-1}$ is very easy to compute). To get 2, we use the fact that $\nabla \cdot \mathbf{U} = 0$ (i.e., we just take the divergence of 1, set equal to zero, and rearrange).

OK, so we have broken our coupled system of 2 equations into 4 equations now:
1. $M\mathbf{U} = -\nabla p$ AKA __MOMENTUM PREDICTOR__. It calculates velocity field $\mathbf{U}$ from pressure field $p$.
2. $H = A\mathbf{U} - M\mathbf{U}$ AKA __H UPDATER__. It calculates $H$ from velocity field $\mathbf{U}$.
3. $\nabla \cdot \left(A^{-1}\nabla p\right) = \nabla \cdot \left(A^{-1}H\right)$ AKA __PRESSURE CORRECTOR__. It calculates pressure field $p$ from $H$.
4. $U = A^{-1}H - A^{-1}\nabla p$ AKA __VELOCITY UPDATER__. It calculates velocity field $\mathbf{U}$ from $H$ and $p$.

A typical PISO loop is: 1->2->3->4->2->3->4. In other words, that's a momentum corrector followed by two inner loops. 

[OpenFOAM's](https://www.openfoam.com/) clean implementation of the PISO algorithm is given below.


```
    Info<< "\nStarting time loop\n" << endl;

    while (runTime.loop())
    {
        Info<< "Time = " << runTime.timeName() << nl << endl;

        #include "CourantNo.H"

        // Update settings from the control dictionary
        piso.read();

        // Pressure-velocity PISO corrector
        {
            #include "UEqn.H"

            // --- PISO loop
            while (piso.correct())
            {
                #include "pEqn.H"
            }
        }

        laminarTransport.correct();
        turbulence->correct();

        runTime.write();

        runTime.printExecutionTime(Info);
    }

    Info<< "End\n" << endl;
```









































Here's the discretization matrix $M$ for a 3x3 mesh.



And here is is for a 4x4 mesh.


