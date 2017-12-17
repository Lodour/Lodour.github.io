title: '[Review] Stochastic Neighbor Embedding'
categories: 数据挖掘
tags:
  - SNE
date: 2017-05-15 18:03:22
---

> 论文 Stochastic Neighbor Embedding
> 作者 Geoffrey Hinton and Sam Roweis
> 机构 Department of Computer Science, University of Toronto
> 会议 NIPS
> 来源 [Stochastic Neighbor Embedding](http://papers.nips.cc/paper/2276-stochastic-neighbor-embedding)

<!--more-->

## 引入

流形学习(Manifold Learning)假设**某些高维数据，实际是嵌入在高维空间中的低维流形**，于是流形学习的目的就是将其映射到低维空间，并在此基础上尽可能保持高维空间中的结构特征。

论文首先总结了当时已经存在的几种方法：

* Multi-Dimensional Scaling (MDS)
通过欧氏距离、某些非线性压缩的距离(nonlinear squashing of distances)或图最短路径(shortest graph paths)来保持不同样本之间的不相似性。

* Isomap
使用图最短路径保持样本之间的不相似性。

* Principal Components Analysis (PCA)
通过原始数据的一种线性投影，使数据保持尽可能大的方差。

* LLE
保持局部的几何性质。

* Self-Organizing Maps
将高维空间的点与低维空间中固定的网格点相联系

* GTM
利用概率对Self-Organizing Maps进行的扩展

以上这些方法，都要求高维空间的对象只和低维空间的一个位置相对应。这样就很难展开“多对一”的映射关系，因为高维空间的对象是可以存在于低维空间的多个分散区域中的。

这篇论文所提出的Stochastic Neighbor Embedding (SNE)算法，在保持最佳邻域关系的基础上将对象映射到低维空间，并且可以很自然地扩展为允许单个对象具有多个不同低维投影的情况。

## 算法

