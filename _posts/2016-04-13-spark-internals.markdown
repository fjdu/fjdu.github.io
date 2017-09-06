---
layout: post
title:  "深入理解 Spark 内部机制"
date:   2016-04-13 Wed 23:41:43
categories: algorithm
---

<h3>
视频链接
</h3>
[A Deeper Understanding of Spark Internals - Aaron Davidson (Databricks)](https://www.youtube.com/watch?v=dmL0N3qfSc8)

<ul>
    <li> 理解 Spark 的运行机制，重点关注性能 </li>
    <li> 执行模型 </li>
    <li> 数据输运 </li>
    <li> 缓存 </li>
</ul>

<ul>
    <li> DAG of RDDs </li>
    <li> DAG 的逻辑执行计划 </li>
    <li> 安排并执行任务 </li>
</ul>
<ul>
    <li> 尽可能地流程化 </li>
    <li> 根据组织数据要求划分执行阶段 </li>
</ul>
<ul>
    <li> Shuffle </li>
    <li> 在不同节点之间重新分布 </li>
    <li> Hash keys into buckets </li>
    <li> 尽量避免 shuffle </li>
    <li> Map side combine; 操作压缩 </li>
    <li> Pull-based, not push-based </li>
    <li> 中间结果写到硬盘 </li>
    <li> 对硬盘上的文件进行筛选处理 </li>
    <li> 每个节点建立 hash map </li>
</ul>
<ul>
    <li> 足够多的划分 </li>
    <li> 尽量减少内存使用 </li>
    <li> 减少数据传输 </li>
    <li> 了解标准库 </li>
</ul>
<ul>
    <li> 划分不能太少也不能太多 </li>
    <li> 一般是 100 - 10000 个 </li>
    <li> 下限：至少是核数的两倍 </li>
    <li> 上限：保证每个任务至少花 100 毫秒 </li>
</ul>
<ul>
    <li> 内存使用问题 </li>
    <li> 性能难以理解地差 </li>
    <li> 机器挂掉 </li>
    <li> PrintGCDetails </li>
    <li> HeapDumpOnOutOfMemoryError </li>
    <li> dmesg </li>
    <li> oom-killer </li>
    <li> 加内存 </li>
    <li> 增加划分 </li>
    <li> 改变程序结构 </li>
</ul>
<ul>
    <li> 显式划分 </li>
    <li> 避免冗余数据结构，避免没必要的中间变量 </li>
    <li> reduceByKey 比 groupBy 好 </li>
</ul>
