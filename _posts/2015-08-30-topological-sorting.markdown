---
layout: post
title:  "Topological sorting"
date:   2015-08-30 Sun 01:23:06
categories: algorithm
---

Ref: [https://en.wikipedia.org/wiki/Topological_sorting](https://en.wikipedia.org/wiki/Topological_sorting)

针对的是有向无循环图。结果不唯一。

算法的思想就是，先找出所有没有入箭头的节点，这些节点组成起始节点集。起始节点集的排序是任意的。

然后从这些起始节点往前一步，得到一个新的节点集合。从这个节点集合里选出没有来自上一步起始节点集之外的入箭头的节点，这些节点组成新的起始节点集。(或者，从起始节点集里任选一个，往前一步，如果这个点没有除了选出的起始节点之外的上家，则把这个点加入起始节点集。)

如此循环下去。

如果进行到某一步剩余的节点数不是零但每个节点都有来自上一步节点之外的节点的入箭头，则意味着这个图没办法排序。
