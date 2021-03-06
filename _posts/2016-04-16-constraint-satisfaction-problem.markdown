---
layout: post
title:  "约束问题"
date:   2016-04-16 Sat 23:14:46
categories: algorithm
---

<b>Ref:</b> Russell & Norvig Chapter 6: Constraint Satisfaction Problems (CSP)

<h2>常用的求解策略</h2>

<ul>
<li>节点相容性：对单个节点的赋值必须在其取值范围内</li>
<li>邻边相容性：一个节点的合法取值不会导致其相邻节点无法取值；一个网络里的每个节点都与其相邻节点相容，则这个网络满足邻边相容性</li>
<li>AC-3：计算邻边相容性的算法；目标是为了减少后续搜索量</li>
<li>路径相容性：指的是两个变量相对第三个变量的相容性；如果这两个变量的每对合法取值都不会导致第三个变量无法取值，则前两个与第三个是路径相容的。</li>
<li>K-相容性：把上面的路径相容性扩展到 K 个</li>
<li>强 K-相容性：不止是 K-相容，而且 (K-1)-相容，(K-2)-相容，...</li>
<li>全局约束：比如要求所有涉及的变量不能取重复值，比如总的资源消耗不能超过多少 (对应边界传递算法，以及相应的边界一致性)，等等</li>
<li>逆向寻踪搜索：深度优先；每次为一个变量赋值，当某个变量没有合法值时，回退，让上一步尝试别的值。</li>
<li>先处理可能取值最少的节点：MRV, minimum-remaining-values</li>
<li>先处理限制最多的节点：degree heuristic</li>
<li>先选取让别的节点可能取值最多的：least-constraining-value</li>
<li>前向检查，邻边相容维持：Maintaining Arc Consistency (MAC)</li>
<li>逆向跳跃：为每个节点维护一个冲突集合；回溯时，直接跳回最近的冲突节点，而不是上一个节点</li>
<li>对于使用了 MAC 策略的前向检查算法而言，简单的逆向跳跃是多余的</li>
<li>冲突导向的逆向跳跃：冲突集合不只是针对一个节点，而是同时针对这个节点之后所有的节点</li>
<li>局部搜索：局部改变变量的值；最少冲突策略，选取与别的变量冲突最少的值；适合在线更新</li>
<li>禁戒搜索：避免重复过去的状态</li>
<li>模拟退火算法</li>
<li>约束权重：不同约束有不同的权重，优先选择最高权重的约束；每个约束的权重会不断调节：被当前赋值违背的那些约束的权重要调高</li>
<li>任务分解，独立子问题：约束图的连通成分，树形 CSP，拓扑排序</li>
<li>分布式约束问题：每个工作代理解决约束问题的一部分</li>
</ul>
