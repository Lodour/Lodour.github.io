title: CodeForces Round 360
categories: ACM
tags:
  - CodeForces
  - ACM
  - 算法
date: 2016-07-17 22:56:23
---

CodeForces Round 360 (div1 + 2)题解与代码

<!--more-->

# A. Opponents
一共有$d$天，每天要面对$n$个敌人，当且仅当所有敌人同时在场时，才会输。

现在给出每天敌人的到场情况，求最大连续赢的天数。

当一天的字符串中含有0时，即胜利。

因此判断每天是否胜利，并累计或清零，同时更新最大值。
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/688A.cpp)

# B. Lovely Palindromes
输入一个很大的整数$n(1\leq n\leq 10^{100000})$，输出第$n$个长度为偶数的回文数字。

很显然，整个回文数字是由前半部分决定的

其大小首先由位数决定，其次由前半部分数字决定。

即由前半部分的数字决定，因此第$n$个偶数长度的回文数字，就是$n$顺过来再倒过来写一遍
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/688B.cpp)

# C. NP-Hard Problem
给出一个有$n$个点和$m$条边的无向图，要求将这些顶点分为两部分

（顶点可以略过而不分配给这两部分）

使得对于任意一条边$\left(u,v\right)$，两个端点处于不同的两部分

只需要对每一个连通分量进行染色，可以使用dfs或并查集实现

略过度为零的点，染色失败则输出-1.
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/688C.cpp)

# D. Remainders Game
给定两个正整数$n$和$k$，以及$n$个正整数$c_i$

对于未知数$x$，若已知$x\%c_i$的值，问能否确定$x\%k$的值。

经过尝试可以发现规律，当$lcm\left(c_1,c_2,...,c_n\right)\equiv 0(mod\ k)$时，可以唯一确定。

简单证明如下，

若不能确定$x\%k$的值，则意味着存在不同的两个数$x_1$和$x_2$，

有$(x_1-x_2)\%c_i=0$而$(x_1-x_2)\%k\neq 0$

此时，$k$不能整除$lcm\left(c_1,c_2,...,c_n\right)$

因此当可以整除时，可以唯一确定（需要进一步证明）

因为会溢出，需要在求解$lcm$时每一步都进行取模操作。
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/688D.cpp)

# E. The values you can make
有$n$个硬币，面值为$c_i$，现在取这些硬币的一个子集，使子集中所有硬币的面值之和为$m$

问对于所有这样的子集$A$，在$A$中取一子集$B$，使得集合$B$中硬币面值之和为$k$

求所有的$k$

很自然想到动态规划，用$dp[i][j][k]$表示前$i$和硬币中，取出子集中面值和为$j$，且合成为$k$的可达性。

1表示可以到达，0表示不可以，很显然有3种情况（假设第$i$个硬币面值为$x$）：

1. 第$i$个硬币，没有放在子集$A$中
$dp[i][j][k]\ =\ dp[i-1][j][k]$
2. 第$i$个硬币，放在子集$A$中，但没有放在子集$B$中
$dp[i][j][k]\ =\ dp[i-1][j-x][k]$
3. 第$i$个硬币，放在子集$A$中，也放在子集$B$中
$dp[i][j][k]\ =\ dp[i-1][j-x][k-x]$

初始状态为$dp[0][0][0]\ =\ 1$
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/688E.cpp)

# F. Dividing Kingdom II
一共$n$个点，$m$条编号为$1...m$的带权边，$q$次询问

每次询问编号在区间$[l,r]$内的边，将这些边的端点分为两部分，求出两个端点处在同一部分的边的权值的最大值

若没有这样的边，则输出-1

将每条边按照边权降序排列，优先将编号在区间内的权值最大的边的两个端点放在不同的部分

当碰到第一条不符合的边时，其边权即为答案

若没有碰到，则答案为-1

具体实现，判断是否处于同一个部分可以利用并查集。
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/687D.cpp)

# G. TOF
强连通分量



