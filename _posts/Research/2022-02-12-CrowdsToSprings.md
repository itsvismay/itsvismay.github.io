---
title: mFem Crowd Energy
date: 2022-04-12 19:00:00 -05:00 
layout: post
tags: research
---

My basic agent energy for agent $a$ is kinetic energy

$$ E(q^a) = \frac{1}{2}m\sum_{i=1}^{i=n}\frac{(x(q_i) - x(q_{i-1}))^2 + (y(q_i) - y(q_{i-1}))^2 + (z(q_i) - z(q_{i-1}))^2}{(t(q_i) - t(q_{i-1}))} $$

with equality constraints on the start and end points

$$A_{eq}q  = b_{eq}$$

with inequality constraints on time intervals and end time

$$Aq = b.$$

To convert this into an mFEM format, we introduce edge lengthsh $s$ and $\lambda$ the lagrange multipliers as a variable.

The continuous energy

$$E = \int Psi(s(q)) - \lambda(q):(s(q) - Q(q))$$

where $Q$ is some functiono that extracts lengths from the positions $q$. 

In the discrete setting

$$E = \sum_i \Psi(s_i) - \lambda:(s_i - Q(q_i)).$$

The updates to the mFEM algorithm are the following:

1. Remove the procruste step
2. Constrained global solve for $q, \lamda$ using PGS
3. Backsolve for s






