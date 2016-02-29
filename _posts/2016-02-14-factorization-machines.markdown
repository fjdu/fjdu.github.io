---
layout: post
title:  "Factorization machines"
date:   2016-02-14 Sun 22:59:48
categories: machine learning
---

Ref:
https://securityintelligence.com/factorization-machines-a-new-way-of-looking-at-machine-learning/

<section>
<h1> A new way of looking at machine learning </h1>
<div>
<ol>
    <li> Introduced by Steffen Rendle 2010 at Google. </li>
    <li> Can be compared to SVM with a polynomial kernel. </li>
    <li> SVM is kind of mystery(?) </li>
    <li> SVM works best on dense data. </li>
    <li> Factorization machines perform well on sparse data. </li>
    <li> Can model \(n\)-way variable interactions. </li>
    <li> Computational complexity can be reduced to linear. </li>
    <li> Optimization methods: stochastic gradient descent, alternating least-squares, Markov chain Monte Carlo (recommended), adaptive gradient descent. </li>
    <li> Widely used in collaborative recommendation systems. </li>
    <li> Recommend music, movies; predict stock market. </li>
    <li> <code> libFM, fastFM, spark-libFM </code> </li>
</ol>
</div>

</section>
