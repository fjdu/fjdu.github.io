---
layout: post
title:  "Can distant galaxies move away from us faster than the speed of light?"
date:   2015-04-11 Sat 12:54:57
categories: physics, cosmology
---

<div>
If we define the speed of a distant galaxy relative to us as
  \[
    v = \dot{a}(t) r,
  \]
where \(a(t)\) is the scale factor, and \(r\) is the comoving coordinate of the
galaxy relative to us, then for sure, with large enough \(r\), \(v\) will be
greater than the light speed \(c\).
</div>
<br>

<div>
This does not violate special relativity, because special relativity only
applies to a local flat patch of the space-time.  The global space-time is made
of such small locally flat patches.
</div>
<br>

<div>
In the following we choose a unit system such that \(c=1\), and define
\(r_\text{c}{}_{}\) such that \(\dot{a} r_\text{c}{}_{} = 1\).
</div>
<br>

<div>
One question: for a distant galaxy with an apparent speed greater than \(c\) at
cosmological time \(t\), is it possible that a photon emitted at \(t\) by that
galaxy can still reach us?
</div>
<br>

<div>
This basically amounts to calculation of future horizon.  Assuming spatial
curvature is zero, we have
\[
  \int_t^\infty \frac{dt'}{a(t')} = r_\text{hor}{}_{},
\]
where \(r_\text{hor}{}_{}\) is the horizon radius.
The question becomes whether \(r_\text{hor}{}_{} > r_\text{c}{}_{}\).  If this
inequality holds, then a photon emitted by the galaxy moving away from us
faster than the speed light can reach us in finite time.
</div>
<br>

<div>
Let \(a(t)\propto t^\alpha\), we have
\[
  \int_t^\infty \frac{dt'}{a(t')} = \frac{1}{1-\alpha} t'^{1-\alpha}|^{\infty}_{t}{}_{}.
\]
For a matter-dominated universe, \(\alpha = 2/3\), hence \(r_\text{hor}{}_{} =
\infty\), meaning that no matter how far the galaxy is, eventually the photons
emitted by it will reach us in finite time --- even if its current apparent
speed is faster than light.
</div>
<br>

<div>
What about accelerated expansion?  Let \(a(t) = a_0 e^{H t}\), then
\[
  r_\text{hor}{}_{} = \int_t^\infty \frac{dt'}{a(t')} = \frac{1}{H a_0} e^{-H t},
\]
and
\[
  r_\text{c}{}_{} = \frac{1}{H a_0} e^{-H t}.
\]
The two are exactly the same.  Hence for such a universe, photons emitted by
galaxies moving away from as with speed higher than the light speed will not
reach us no matter how long we wait.
</div>
<br>

<div>
See also:
<ul>
<li> Stuckey 1991: http://users.etown.edu/s/stuckeym/AJP1992a.pdf </li>
<li> Davis and Lineweaver 2001: http://arxiv.org/pdf/astro-ph/0011070v2.pdf </li>
</ul>
</div>
