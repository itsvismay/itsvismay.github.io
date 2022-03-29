---
title: Converting crowds to springs
date: 2022-02-12 19:00:00 -05:00 
layout: post
tags: research
---

My basic agent energy is kinetic energy

$$ C^K = \frac{1}{2}m \sum_{i=0}^{i=n}\frac{\Delta x_i^2 + \Delta y_i^2 + \Delta z_i^2}{\Delta t_i} $$

in a constrained optimization

$$ s* = argmin \; C^K \\ 
		x_0, y_0, t_0 = given\\
		x_n, y_n = given
		st. \; t_n \leq MaxTime $$


Spring energy is 

$$ C^S = \frac{1}{2}k\sum_{i=0}^{i=n}(\Delta x^2 + \Delta y^2 + \Delta z^2 + \Delta t^2) $$

nonzero spring

$$ \frac{1}{2}k \sum_{i=0}^{i=n}(\Delta x - l)^2 \\
x_0, y_0, t_0 = given\\
x_n, y_n, t_n = given$$ 

