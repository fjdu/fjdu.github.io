---
layout: post
title:  "Rolling hash"
date:   2015-04-02 Thu 14:05:31
categories: algorithm
---

<div>
  Definition:
  $$
  h_i = s[i]\;p^0 + s[i+1]\;p^1 + s[i+2]\;p^2 + \cdots s[i+n]\;p^n.
  $$
  Hence
  $$
  h_{i+1}{}_{} = s[i+1]\;p^0 + s[i+2]\;p^1 + s[i+3]\;p^2 + \cdots s[i+n+1]\;p^n.
  $$
  We have
  $$
  h_{i+1}{}_{} = (h_i - s[i]) / p + s[i+n+1] p^n,
  $$
  which means that the new hash can be done in \(O(1)\) time, rather than in
  \(O(n)\) time.
</div>

References:

1. [Quora](https://www.quora.com/What-is-a-rolling-hash-and-when-is-it-useful)
