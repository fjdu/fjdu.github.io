---
layout: post
title:  "Scaling of flux per beam"
date:   2015-05-03 Sun 15:48:16
categories: astronomy
---

\\[
  F \equiv \int_\text{beam} S(\vec{n}) P(\vec{n}) d\Omega {}_{}.
\\]

Let
\\[
  P(\vec{n}) \propto \exp\left[ -\frac{1}{2}\left( \frac{r}{\sigma} \right)^2 \right],
\\]
and
\\[
  S(\vec{n}) \propto r^\alpha,
\\]
we have
\\[
  F \propto \int r^\alpha \exp\left[ -\frac{1}{2}\left( \frac{r}{\sigma}
  \right)^2 \right] r dr \propto \Gamma\left(\frac{\alpha}{2}+1\right) \sigma^{\alpha+2}.
\\]

So we can scale the flux with \\(\sigma^{\alpha+2}\\).
