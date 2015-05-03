---
layout: post
title:  "Find out the top elements of a big list"
date:   2015-04-09 Thu 17:15:22
categories: algorithm
---

<div>
  Let's say, you have \(10^{11}\) numbers stored in the hard drive, and we want
  to find out the largest 100 of these numbers.  The value range of these
  numbers is not known and can be arbitrary.
  <p>
  Assume the memory can hold \(10^8\) numbers, then we could divide the
  original \(10^{11}\) numbers into 1000 blocks, each containing \(10^8\)
  numbers.  Then we sort the \(10^8\) numbers in the first block, and take out
  the top 100 numbers.  Next we replace the first 100 numbers of the second
  block by the pairwise larger of them and the top 100 numbers of the first
  block, and sort the second block.  Continue until we finish with the last
  block.
  <p>
  The number of computation is of the order of
  \[1000 \times \left(100 + 10^8 \times \ln\left(10^8\right)\right),\]
  which is not bad.
  <p>
  In the general case, let the number of elements to be \(N\), the memory size
  to be \(M\), and the number of top elements we want to find to be \(T\), then
  the number of computation is
  \[\frac{N}{M} \times \left(T + M \times \ln\left(M\right)\right).\]
  Actually, when \(M = T\), the number of computation is the lowest, and is
  linear in \(N\), but then we have to consider the penalty from frequent disk
  IO.
</div>
