---
layout: post
title:  "Semaphores"
date:   2016-03-06 Sun 21:50:06
categories: algorithm
---

<h3>
原文链接
</h3>
[Operating Systems: Three Easy Pieces](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-sema.pdf)

Semaphore: 信号量，旗语

<p>
需要锁和条件变量解决大量的并发问题。Edsger Dijkstra 是最初认识到这一点的人之一 (尽管准确的历史难以得知)，他引入了叫做 semaphore 的同步化元件。Semaphore 可以被同时用作锁和条件变量。
</p>

<p>
<b>定义</b>
Semaphore 是一个取整数值的对象，并且可以用两个程序操纵这个值。在 POSIX 标准里，两个程序叫做 sem_wait() 和 sem_post()。由于 Semaphore 的初值决定了它的行为，在调用任何涉及到 semaphore 的程序之前，必须做初始化。
</p>
