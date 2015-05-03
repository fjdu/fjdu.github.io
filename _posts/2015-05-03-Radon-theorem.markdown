---
layout: post
title:  "Radon's theorem"
date:   2015-05-03 Sun 12:41:42
categories: algorithm, math
---

Any set of \\(d+2\\) points in \\(R^d\\) can be partitioned into two disjoint
sets whose convex hulls intersect.

In two dimension, this means that given four points, you can find a way to
classify them into two classes so that the two classes cannot be separated by a
single straight line.  This is related to the concept of [Vapnik-Chervonenkis
dimension](http://en.wikipedia.org/wiki/VC_dimension).

The VC dimension quantify the capacity of a statistical classification
algorithm.  The larger the VC dimension is, the more capable it is to classify
complicated dataset.

------------------------

# Proof

The \\(d+2\\) points cannot be linearly independent.  Furthermore, we can
choose the coefficients so that the sum of them is zero.  This is possible
because the system of linear equations is under-determined.

Classify the \\(d+2\\) points based on whether the corresponding coefficient is
positive or negative.  Then using these coordinates we can construct a point
that is a convex combination of points of either class.

This point must belong to the convex hull of each class.  Q.E.D.

Ref: [Wikipedia](http://en.wikipedia.org/wiki/Radon%27s_theorem)
