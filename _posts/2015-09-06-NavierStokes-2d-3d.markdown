---
layout: post
title:  "The difference between 2D and 3D Navier-Stokes equations"
date:   2015-09-06 Sun 09:29:09
categories: physics, math, fluid mechanics
---

From: [https://www.youtube.com/watch?v=DgmuGqeRTto](https://www.youtube.com/watch?v=DgmuGqeRTto)

<div>
Consider an \(n\)-dimensional ball of fluid with size scale \(L\).  The total kinetic energy within such a ball is of the order of

$$ v^2 L^n. $$

Assuming the evolution of the fluid ball consume energy, we have

$$ v \propto L^{-n/2}. $$

Thus the Reynolds number defined as

$$ \text{Re} = \frac{v L} {\nu} $$

will be

$$ \text{Re} \propto L^{1-n/2}. $$

Hence, for \(n=2\), \(\text{Re}\) does not depend on the length scale, which means that turbulence will not be amplified at small scale.

For \(n=3\), \(\text{Re}\propto L^{-1/2}\), which means that turbulence becomes stronger at small scale.
</div>
