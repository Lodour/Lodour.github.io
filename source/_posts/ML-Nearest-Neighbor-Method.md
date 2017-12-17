title: 机器学习总结：近邻法
categories: 算法
tags:
  - 机器学习
  - 近邻法
date: 2017-01-22 14:24:15
---

最近邻法、k-近邻法和近邻法的快速算法。

<!--more-->

## 最近邻法
> 最近邻法源于这样一种直观的想法：
> 对于一个新样本，把它逐一与已知样本比较，找出距离新样本最近的已知样本，以该样本的类别作为新样本的类别。

已知具有$N$个样本的$c$类样本集$S\_N=\\{({\bf x}\_i, \omega\_j)|1\le i\le N, 1\le j\le c\\}$，其中${\bf x}\_i$是样本$i$的$k$维特征列向量$(x\_1, x\_2, ..., x\_k)^T$，$\omega\_i$是样本对应的类别，定义两个样本间的距离度量$\delta({\bf x}\_i, {\bf x}\_j)$。

1. 若采用欧式距离，则有$\delta({\bf x}\_i, {\bf x}\_j) = \sqrt{({\bf x}\_i-{\bf x}\_j)^T({\bf x}\_i-{\bf x}\_j)}$

2. 若采用马氏距离，则有$\delta({\bf x}\_i, {\bf x}\_j) = \sqrt{({\bf x}\_i-{\bf x}\_j)^T\Sigma^{-1}({\bf x}\_i-{\bf x}\_j)}$，其中$\Sigma$是样本集$S\_N$的协方差矩阵。

对于未知样本${\bf x}$，将其决策为$\omega\_j$类当且仅当${\bf x}\_i\in\omega\_j$，其中$i=\mathop{\arg\min}\limits\_{i=1,2,...,N}{\delta({\bf x}, {\bf x}\_i)}$

则$\omega\_j$类的判别函数可以写成：

$$
g\_i({\bf x}) = \mathop{\arg\min}\limits\_{ {\bf x}\_j\in\omega\_i}\delta({\bf x}, {\bf x}\_j), i=1,2,...,c
$$

同时，决策规则为各类判别函数的大小，即：

$$
{\bf x}\in\omega\_k, k = \mathop{\arg\min}\limits\_{i=1,2,...,c}g_i({\bf x})
$$

以Iris数据集为例，为了便于画图，仅选取两个特征。
![NN for Iris](nn.png "NN for Iris")

## K-近邻法
与最近邻法类似，但是选取前$K$个近邻，若其中有$k\_i$个属于$\omega\_i$类，则$\omega\_i$类的判别函数是：
$$
g\_i({\bf x})=k\_i, i=1,2,...,c
$$

决策规则是：
$$
{\bf x}\in\omega\_k, k = \mathop{\arg\min}\limits\_{i=1,2,...,c}g_i({\bf x})
$$

同样，以Iris数据集为例。
![kNN for Iris](knn.png "kNN for Iris")

## 近邻法的快速算法
[参考](http://202.197.191.206:8080/30/text/chapter03/3_4_3.htm)
