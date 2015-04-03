---
layout: post
title:  "Stochastic time derivative"
date:   2015-04-03 Fri 10:11:00
categories: math
---

<div>
  Definition:
  $$
  \frac{dx}{dt} = \eta(t),
  $$
  where \(\eta(t)\) satisfies
  $$
  \langle \eta(t)\eta(t')\rangle = \sigma^2 \delta(t-t').
  $$
  We want to know \(\langle dx^2\rangle\) over an infinitesimal time interval
  \(dt\).

  To calculate this, we further divide \(dt\) into \(N\) equal parts, where
  \(N\) is a large number.  We have
  $$
  dx = \left[\eta(dt/N) + \eta(2dt/N) + \cdots + \eta(dt)\right] \frac{dt}{N}.
  $$
  We assume the \(\eta\)s in the bracket are independent and identically
  distributed.  This gives
  $$
  \langle dx^2\rangle
  = \left[\eta^2(dt/N) + \eta^2(2dt/N) +
    \cdots + \eta^2(dt)\right] \left(\frac{dt}{N}\right)^2 \\
  = N \frac{\sigma^2}{dt/N} \left(\frac{dt}{N}\right)^2
  = \sigma^2 dt.
  $$
  Here we have used
  $$
  \eta^2(dt/N) \approx \frac{\sigma^2} {dt/N}.
  $$

  We could have assumed \(\eta^2 dt = \sigma^2\) from the beginning, but the
  mental image is a bit different.  In \(\eta^2 dt\), we imagine \(dt\) is
  still somehow "large", although it is an infinitesimal, while for \(dt/N\),
  we assume it is an unsplittable "atomic" time interval, within which \(\eta\)
  is constant (not fluctuating).  Namely, we were somehow choosing \(N\) such
  that \(dt/N\) equals exactly the "natural" "atomic" time.  In reality, the
  "atomic" time may be the shortest time a system variable can change its
  value.  This is reasonable, because we can't imagine something really happens
  within an infinitely small amount of time.  Everything takes a nonzero amount
  of time to happen.
  <p>
  And we have seen that discretization makes things solid and clear.
  </p>
</div>
