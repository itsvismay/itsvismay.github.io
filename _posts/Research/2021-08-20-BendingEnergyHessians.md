---
title: Etienne Vouga's Bending Energy Hessian
date: 2021-08-20 19:00:00 -05:00 
layout: post
tags: research
---
# Energy and Gradients
This is given in Etienne Vouga's notes here: 

https://www.cs.utexas.edu/users/evouga/uploads/4/5/6/8/45689883/turning.pdf

# Bending Energy Hessian 
For a minimal example of a 2 segment rod with 3 vertices $x_{i-1}, x_i, x_{i+1}$, bending energy is:

$$E(\theta (v1(x_i, x_{i-1}), v2(x_{i+1}, x_i))) = 0.5*K*(\theta(v1, v2) - \theta_0)^2$$

Since our rods are meant to be as straight as possible, $\theta_0 = 0$. This assumption simplifies the energy and gradients to the following:

$$E = 0.5*K*(\theta(v1, v2))^2$$

This comma notation below means derivative $\frac{d f}{dx} = f_{,x}$ and I didn't know what the comma notation for 2nd derivative was, so I use 2 commas $f''(x) = f_{,x ,x}$. Also I drop the $v1, v2$ since they're just functions of $x$ :
| Jacobians |
|:-|
|$\bold{E}_{,i-1} = \frac{dE}{d x_{i-1}} = K*\theta(\cdot)*\theta_{,i-1}$|
|$\bold{E}_{,i} = \frac{dE}{d x_{i}} = K*\theta(\cdot)*(\theta_{,i}) = K*\theta(\cdot)*(-\theta_{,i-1} - \theta_{,i+1})$|
|$\bold{E}_{,i+1} = \frac{dE}{d x_{i+1}} = K*\theta(\cdot)*\theta_{,i+1}$|

Then the $9x9$ symmetric hessian matrix is made up of 9 individual $3x3$ blocks which are:

| 9 x 9 | Hessian | Structure |
|:---|:---|:---|
| $\bold{E}_{,i-1, i-1}$ | $\bold{E}_{,i-1, i}$ | $\bold{E}_{,i-1, i+1}$ |
| $\bold{E}_{,i, i-1}$ | $\bold{E}_{,i, i}$ | $\bold{E}_{,i, i+1}$ |
| $\bold{E}_{,i+1, i-1}$ | $\bold{E}_{,i+1, i}$ | $\bold{E}_{,i+1, i+1}$ |

-----------

|First row|
|:---|
| $\bold{E}_{,i-1, i-1} = K*\theta_{,i-1}*\theta_{,i-1}^T + K *\theta *\theta_{,i-1, i-1}$ |
| $\bold{E}_{,i-1, i} = K*\theta_{,i-1}*\theta_{,i}^T + K *\theta *\theta_{,i-1, i}$ |
| $\bold{E}_{,i-1, i+1} = K*\theta_{,i-1}*\theta_{,i+1}^T + \sout{K*\theta *\theta_{,i-1, i+1}} = 0$ |
| $\bold{E}_{,i, i-1} = H_{,i-1, i}^T$|
| $\bold{E}_{,i, i} = K*\theta_{,i}*\theta_{,i}^T + K *\theta *\theta_{,i, i} \newline = K*(-\theta_{,i-1} - \theta_{,i+1})*(-\theta_{,i-1} - \theta_{,i+1})^T + K*\theta*(-\theta_{,i-1,i} - \theta_{,i+1,i})$|
|$\bold{E}_{,i, i+1} = K*\theta_{,i+1}*\theta_{,i}^T + K *\theta* \theta_{,i+1, i}$|
| $\bold{E}_{,i+1, i-1} = E_{,i-1, i+1}^T$|
| $\bold{E}_{,i+1, i} = E_{,i, i+1}^T$|
| $\bold{E}_{,i+1, i+1} = K*\theta_{,i+1}*\theta_{,i+1}^T + K *\theta* \theta_{,i+1, i+1}$|

----------
Now I will define each individual component and jacobian from above:

|Components|
|:-|
|$v_1 = x_i - x_{i-1}$|
|$v_2 = x_{i+1} - x_{i}$|
|$z = \frac{v_1 \times v_2}{max(\|v_1 \times v_2\|, \epsilon)}$|
|$[z]$ cross product matrix of $z$
----- 


|Jacobians of Theta|
|:---|
|$\frac{d\theta}{d x_{i-1}} = \frac{-(x_i - x_{i-1}) \times \hat z}{\| x_i - x_{i-1} \|^2} = \frac{-v_1 \times \hat z}{\| v_1 \|^2}$|
|$\frac{d \theta}{d x_{i+1}} = \frac{-(x_{i+1} - x_{i}) \times \hat z}{\| x_{i+1} - x_{i} \|^2} = \frac{-v_2 \times \hat z}{\| v_2 \|^2}$|
|$\frac{d \theta}{d x_{i}} = (-\frac{d \theta}{d x_{i-1}} - \frac{d \theta}{d x_{i+1}})$|
-----

|Hessians of Theta|
|:-|
| $\frac{d^2\theta}{d x_{i-1} ^2} = \frac{\|v_1 \|^2 [z] - 2v_1 (v_1 \times \hat z)^T}{\|v_1\|^4}$ |
| $\frac{d^2\theta}{d x_{i+1} ^2} = \frac{\|v_2 \|^2 [z] + 2v_2 (v_2 \times \hat z)^T}{\|v_2\|^4}$ |
| $\frac{d^2\theta}{d x_{i-1} dx_i} = \frac{\|v_1 \|^2 [z] + 2v_1 (v_1 \times \hat z)^T}{\|v_1\|^4}$ |
| $\frac{d^2\theta}{d x_{i+1} dx_i} = \frac{\|v_2 \|^2 [z] + 2v_2 (v_2 \times \hat z)^T}{\|v_2\|^4}$ |
| $\frac{d^2\theta}{d x_i dx_i} =-\frac{d^2\theta}{dx_{x-1} dx_i} - \frac{d^2\theta}{dx_{x+1} dx_i}$ |
