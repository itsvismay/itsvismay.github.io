---
title: Notes on Modal Derivatives
date: 2018-04-08 19:00:00 -05:00 
layout: post
tags: research
---

## Based on:
Backgroun on modal analysis is in Real-Time Subspace Integration for StVK Deformable Models [Barbic-2005] and Part 2 of the Siggraph2012 Course Notes on [femdefo.org]. I'll try to provide a more some intuition about why it works, and a more thorough derivation. I'm using the same notation and figures as in the course notes on [femdefo.org].


## Modal Analysis
Requires you to solve the general eigenvalue problem in order to find the eigenvalues $\lambda$ and eigenvectors $x$ of the system.

$$Kx = \lambda Mx \tag{1}$$

The eigenvals $\lambda$ determine the energy state of the object's transformation described in $x$. $\lambda_i = 0$ means $x_i$ describes a rigid transformation.

For model reduction, we want low energy states of the object, so we select $r << n$ of the smallest $\lambda_i$ where $0<i<r$ and create a reduced transformation basis $\psi = [x_0, x_1, ..., x_r]$.

When the object is wiggled, it tends to move into the low energy states, so our reduced  transformation basis $\psi$ should capture most of the object's range of motion. If deformation is really big, it would have a high energy, and its motion wouldn't be captured by $\psi$. Thats why modal analysis doesn't work for large deformations.

This is usually computed at the object's rest position, but it can be done at other positions too. As long as the displacements are small, you can compute any deformation around the position at which you compute the basis $\psi$ as a linear combination of $\psi_0, \psi_1,...\psi_r$. So for any small displacement $u$

$$u = u(p) = \sum_{i=0}^r \psi[,i] p[i] \tag{17}$$

where $p_i$ is the weight (or strength) of $\psi_i$ needed for that $u$. This equation (eq 17) shows modal analysis is just the *first order* taylor expansion of $u(p)$ around the object's rest position. Thats why the deformations described by this basis are similar to linear elasticity. Basis $\psi$ is calculated on a constrant $K$ at rest, and as the deformations get further away from rest, their accuracy decreases.

## Modal Derivatives
captures the second order taylor approximation term in function $u(p)$. So the deformations are more accurate even if you go further away from rest position.

$$u = u(p) = \sum_{i=0}^r \psi[,i] p[i] + \sum_{i=0}^r\sum_{j=0}^r \frac{1}{2}\phi[i,j]p[i]p[j] \tag{17}$$


In the equation above, term $\psi_i$ corresponds to the first derivative of $u$ w.r.t $p$, $\frac{du}{dp}$ and the term $\phi_{ij} = \frac{d^2u}{dp^2}$. This is just from the definition of a taylor expansion. We already know what the $\psi$ terms are, we now need to find the $\phi$ terms.

There's two ways to do this. Both are included in Jernej Barbic's [thesis], but I prefer the method described in the SIGGRAPH course notes.

Since any small deformation around rest can be described as $u(p)$ there is some force $f(u(p))$ that will get the object to that deformation. Choose the force to be 

$$f(u(p)) = M\psi \Lambda p \tag{13}$$

where $\Lambda$ is a diagonal matrix of eigenvalues. This equation describes the force for any small deformation. 

$$\frac{df}{du}\frac{du}{dp} = M \psi \Lambda \tag{14}$$

Take the derivative of (13) to move towards finding $\frac{d^u}{dp^2}$. Couple of things to note in (14): $\frac{df}{du}=K(u(p))$ the stiffness matrix and $M \psi \Lambda$ is a constant w.r.t $p$.

Taking the derivative of 14, and being a little (very) careless with notation ($\frac{du}{dp_i}$ should all be labeled properly) we get

$\frac{df}{du}\frac{d^2u}{dp^2} + \frac{dK}{dp}\frac{du}{dp} = 
\frac{df}{du}\frac{d^2u}{dp^2} + \frac{dK}{du}\frac{du}{dp}\frac{du}{dp} = 0 $

where $\frac{dK}{du} = H$ is the Hessian matrix **w.r.t forces**.

$$K\frac{d^2u}{dp_i dp_j} = -H\frac{du}{dp_i}\frac{du}{dp_j} = -(H:\frac{du}{dp_i})\frac{du}{dp_j}$$

Since H is a rank 3 tensor, doing a double dot product (:) with a vector results in a rank 2 matrix. Also, remember that $\frac{du}{dp_i} = \psi_i$

$$-(H:\frac{du}{dp_i})\frac{du}{dp_j} = \sum_{ij} H\psi[i] \cdot \psi[j] = v_{ij}$$

where $H\psi[i]$ is a rank 3 sensor multiplied by a vector, which results in a matrix. When multiplied by another vector $\psi[j]$ you'll get a vector $v_{ij}$. The last step is to multiply the vector by $K^{-1}$

$$K^{-1} K\frac{d^2u}{dp_i dp_j} = K^{-1} v_{ij} = \frac{d^2u}{dp_i dp_j} = \phi_{ij}$$

Once you have the modal derivatives $\frac{d^2u}{dp_i dp_j}$, you need to do another Mass-PCA to find a linearly independent basis because $\phi$ are not linearly independent. 

This is all explained further in the Real Time Subspace... paper ([Barbic-James-2005]) and in the course notes on [femdefo.org].





[Barbic-James-2005]: https://www.ri.cmu.edu/pub_files/pub4/barbic_jernej_2005_1/barbic_jernej_2005_1.pdf
[femdefo.org]: femdefo.org
[thesis]: http://www-bcf.usc.edu/~jbarbic/thesis-barbic.pdf
