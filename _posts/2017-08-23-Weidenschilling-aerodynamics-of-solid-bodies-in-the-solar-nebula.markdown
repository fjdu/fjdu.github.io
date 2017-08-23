---
layout: post
title:  "原行星盘中固体的空气动力学"
date:   2017-08-23 Wed 11:07:21
categories: astrophysics
---

作者：[S. J. Weidenschilling](http://adsabs.harvard.edu/cgi-bin/nph-abs_connect?db_key=AST&db_key=PRE&qform=AST&arxiv_sel=astro-ph&arxiv_sel=cond-mat&arxiv_sel=cs&arxiv_sel=gr-qc&arxiv_sel=hep-ex&arxiv_sel=hep-lat&arxiv_sel=hep-ph&arxiv_sel=hep-th&arxiv_sel=math&arxiv_sel=math-ph&arxiv_sel=nlin&arxiv_sel=nucl-ex&arxiv_sel=nucl-th&arxiv_sel=physics&arxiv_sel=quant-ph&arxiv_sel=q-bio&sim_query=YES&ned_query=YES&adsobj_query=YES&aut_logic=OR&obj_logic=OR&author=Weidenschilling%2C+S&object=&start_mon=&start_year=&end_mon=&end_year=&ttl_logic=OR&title=&txt_logic=OR&text=&nr_to_return=200&start_nr=1&jou_pick=ALL&ref_stems=&data_and=ALL&group_and=ALL&start_entry_day=&start_entry_mon=&start_entry_year=&end_entry_day=&end_entry_mon=&end_entry_year=&min_score=&sort=SCORE&data_type=SHORT&aut_syn=YES&ttl_syn=YES&txt_syn=YES&aut_wt=1.0&obj_wt=1.0&ttl_wt=0.3&txt_wt=3.0&aut_wgt=YES&obj_wgt=YES&ttl_wgt=YES&txt_wgt=YES&ttl_sco=YES&txt_sco=YES&version=1)

来源：[Aerodynamics of solid bodies in the solar nebula](http://adsabs.harvard.edu/abs/1977MNRAS.180...57W)

# 摘要

<p>
气体的压力梯度导致转动速度低于自由轨道速度，拖拽力让固体的轨道衰减。本文通过解析和数值方法研究了各种拖拽力规律下的运动。最大径向速度与拖拽力规律无关，并且对原行星盘的质量不敏感。径向速度对颗粒的大小敏感，对于米量级的固体，速度达到 \(10^4\) cm/s 的量级。可能的后果包括：
<ul>
<li>原行星盘中短时标内固体物质的混合</li>
<li>碰撞导致行星星子的快速累积</li>
<li>固体物质的尺度和密度的分化</li>
<li>某些区域可能会组分异常</li>
</ul>
</p>

# 1. 引言

> Cosmogony: 天体演化学，研究宇宙起源，偏向于太阳系的起源

- 中心凝聚的扁盘形态，盘中层的压强、密度、温度随半径减小
- 气体的压力梯度导致其转动速度低于自由轨道速度
- 固体不受压力梯度的支撑，因此速度与气体速度不同，于是气体对固体有拖拽力，导致固体的轨道衰减

- Kiang (1962): 考虑了椭圆轨道，但假定了介质是静态且以 Kepler 速度旋转的
- Whipple (1964, 1972, 1973): 考虑到了非 Keplerian 旋转，计算了很小和很大物体两种极限，使用了 Stokes 和 Epstein 拖拽规律
- Cameron (1973): Whipple 工作的后续

本文扩展了 Whipple 的工作。

# 2. 基本关系

<div hidden>
\(
\newcommand\Vk{V_\text{k}}
\newcommand\Vg{V_\text{g}}
\)
</div>

<p>
令 \(P\) 为气体压强，\(\rho\) 为其密度，\(r\) 为径向距离。引力加速度 \(g\) 与 Kepler 轨道速度 \(\Vk\) 的关系是
\[
  g = \frac{GM_\odot}{r^2} = \frac{\Vk}{r},
\]
</p>

<p>
在与气体同步旋转的参考系内，剩余引力是
\[
  \Delta g = \frac{1}{\rho}\frac{dP}{dr}.
\]
我们只考虑中心凝聚的情形，也就是说 \(dP/dr\) 和 \(\Delta g\) 都是负的。以 \(\Vg\) 记气体的转动速度，则
\[
  \frac{\Vg^2}{r} = \frac{\Vk^2}{r} + \Delta g;
\]
若 \(\Delta g/g\ll1\)，则
\[
  \Delta V = \Vk - \Vg \simeq -\left(\frac{\Delta g}{2g}\right) \Vk
\]
是气体速度相对自由轨道速度的偏离。
</p>

<p>
用幂律近似压力分布函数
\[
  P = P_0 (r/r_0)^{-n}.
\]
理想气体：
\[
  \Delta g = \frac{1}{\rho} \frac{dP}{dr} = \frac{-nRT}{\mu r}.
\]
</p>
