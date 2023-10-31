---
title: Approximating Stiffness Accurately
date: 2022-12-13 19:00:00 -05:00 
layout: post
tags: research
---

A common way to create a reduced DOF approximation of an FEM elastic simulation is by doing modal analysis.

Modes are computed by solving the eigenvalue problem
$$Kx = \lambda M x$$
and selecting the smallest $m$ eigenvalues $\lambda_{0..m}$ and associated eigenvectors $v_{0..m}$.

These eigenvectors form a basis for motion $U$ such that the previous DOFs $x$ are approximated by $x(z) = Uz$.

The reduced simulation for this mesh can be written as a function $P(z, U, M, K, f_{ext}, \Delta t)$ with inputs for reduced DOFs $z$, the modal basis $U$, mass $M$, stiffness $K$, external forces and timestep $\Delta t$.

## Problem 
Lets say you want to simulate a very finely tetrahedralized object at a particular target YM $E_t$. Naively this is computationally intractable, so you reduce the problem through modal analysis. It is understood that the behavior of the reduced mesh during simulation is artificially stiffer than the behavior of the unreduced mesh and the effective YM $E_e$ only approaches $E_t$ in the limit as the number of modes $m$ approaches $n$. 

Homogenizing the stiffness values of the original and reduced mesh requires finding a new YM $E_i$ such that the effective stiffness of the reduced mesh is equal to the target stiffness. This can be written as an optimization problem for the input YM which results in an effective YM that matches the target YM

$$E_i^* = argmin |E_e(E_i, m) - E_t|^.2$$

We assume $E_t$ is known, $E_e$ is derived from the results of running the simulation $P$ on the reduced model and using inverse harmonics (Mandelshtam & Taylor 1997).

Maybe the whole function $E_e$ can be taught to a network? That would make this whole thing super fast.





