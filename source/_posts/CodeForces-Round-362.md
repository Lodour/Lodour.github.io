title: CodeForces Round 362
categories: ACM
tags:
  - CodeForces
  - ACM
  - 算法
date: 2016-07-19 19:38:06
---

CodeForces Round 362 (div1 + div2)题解与代码

<!--more-->

# A. Pineapple Incident
一只狗会在$t+ks(k\geq 0)$和$t+ks+1(k>0)$时间叫

输入时间$x$，判断其是否在叫

直接减去$t$后对$k$取模判断即可

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697A.cpp)

# B. Barnicle

输入一个以格式$a.deb$表示的浮点数，将其转换为普通格式

要求没有多余的前缀和后缀0，以及其他的一些要求

输入后找到小数点位置和$b$的值，依次后移并补0即可

输出前对前后缀0以及小数点进行判断

需要注意类似`1.0e0`的数据

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697B.cpp)

# C. Lorenzo Von Matterhorn

一颗树，以下两种操作：

1. $u$到$v$的每条边的权值增加$w$
2. 输出$u$到$v$的权值和

利用map记录每个结点到其父亲结点的边的权值，分别走到两点的最近公共祖先并进行处理即可。

根据结点的编号规律，编号除以2即为父亲结点，进而求解LCA就很简单了

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697C.cpp)

# D. Puzzles

一棵树，等概率改变每个结点的儿子结点的顺序，求出每个节点dfs次序的期望值。

首先可以想到，每个结点的期望，等于其父亲结点的期望，加上其余兄弟次序累加的平均值

即假设每个兄弟所在子树的结点总数为$p_1,p_2,p_3...p_k$

则$p_1$结点期望值的增量应为$1.0+0.5*\sum_2^kp_i$

最初做法是一次dfs求出每个结点子树的结点总数，第二次dfs进行dp。

[C++代码1](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697D1.cpp)

之后参考别人代码，发现漏看了条件，即父亲结点的编号一定小于儿子结点

因此可以不用dfs，从最大的结点开始往前统计即可

另一方面，每个兄弟位于某结点之前的概率均为0.5，也即对期望增量的贡献为$0.5*P_i$

同样，dp时从最小的结点开始即可。

[C++代码2](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697D2.cpp)

# E. PLEASE

三个杯子，初始时中间的杯子里放了一个东西，进行$n$轮操作

每次操作等概率选择两边的杯子，与中间的杯子交换

问最后东西在中间杯子的概率，以$p/q$的形式表示

要求$p$与$q$互素，输出$p,q$对$10^9+7$取模后的结果

直接推公式，发现化简后的分子与分母一定是互素的
$$
ans =
\begin{cases}
p=\frac{2^{n-1}+1}{3},q=2^{n-1} & \text{if $n$ is even} \\\\
p=\frac{2^{n-1}-1}{3},q=2^{n-1} & \text{if $n$ is odd}
\end{cases}
$$

而$n$很大，以其分解因式的形式输入，即输入$p_i$，则$n=\prod_{i=1}^kp_i$

第一次是判断出奇偶，分别计算答案，利用费马小定理对幂取模进行计算

[C++代码1](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697E1.cpp)

之后借鉴别人代码，发现原公式中的$(-1)^n$也可以利用快速模幂进行计算，即$(mod-1)^n$

因此分别计算出$2^{n-1}$以及$(-1)^n$的值，相加并乘3的逆元即可

[C++代码2](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697E2.cpp)

# F. Legen...

给出$n$个单词，以及每个单词的权值，要求求出长度最大为$l$的一个字符串

这些单词在字符串中出现一次则累加一次权值，求最大权值和

直接建立AC自动机后，利用矩阵$mat[i][j]$表示从状态$i$转移一次到状态$j$的最大权值

则$mat^2$为转移两次，只需求出转移$l$次后的最大权值即为答案

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/697F.cpp)
