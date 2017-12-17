title: CodeForces Round 361
date: 2016-07-17 02:10:26
categories: ACM
tags:
	- CodeForces
	- ACM
	- 算法
---

CodeForces Round 361 (div.2)解题思路与代码

<!--more-->

## A. Mike and Cellphone
给出一串数字序列，询问在手机数字键盘上能否唯一确定起始位置。
> 1 2 3
> 4 5 6
> 7 8 9
> \# 0 \#

根据以上数字分布，若不能唯一确定，则给出的数字序列在键盘上一定是可移动的：

1. 向上移，则不能含有123
2. 向下移，则不能含有709
3. 向左移，则不能含有1470
4. 向右移，则不能含有3690

若不可移动，则唯一确定，输出`YES`，否则输出`NO`.

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689A.cpp)

## B. Mike and Shortcuts

规定$i$到$j$的花费为$\left|i-j\right|$，同时给出$n$个数字$a_i$，表示$i$到$a_i$的花费为1，求出点1到每一点的最短路。

实际上，只需要添加三种边:$(i,i+1)$, $(i+1,i)$, $(i,a_i)$

由于这三种边的长度均为1，因此可以直接用bfs进行搜索，也可以用单源最短路的模板。

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689B.cpp)

## C. Mike and Chocolate Thieves

给出$m(1\leq m\leq10^{15})$的值，求最小的$n$，使得区间$[1,n]$所包含的有序数对$(i,ik,ik^2,ik^3)$的个数为$m$.

首先想到，对于确定的$n$，可以在$O(\sqrt[3]{n})$内求出包含有序数对的个数，并且个数随$n$递增，因此考虑对$n$进行二分。需要注意的是，二分的上界$R$不是$m$的上界，这里很容易出错，需要设置得大一些。

[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689C.cpp)

## D. Friends and Subsequences

给出两个长度为$n(1\leq n\leq2\*10^5)$的序列$a$和$b$，求出所有区间$[l,r]$的个数，

使得$\max\lbrace a_l...a_r\rbrace=\min\lbrace b_l...b_r\rbrace$

也即$\max\lbrace a_l...a_r\rbrace-\min\lbrace b_l...b_r\rbrace=0$

对于确定的下界$L$，对上界$R$进行二分，分别找到$rMin$和$rMax$，使得区间$[L,rMin]$到区间$[L,rMax]$均满足等号。

在求解区间最小/大值时，可以使用线段树或RMQ。

[C++代码#1](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689D1.cpp)

其实还可以用两个单调队列，维护区间最大值和最小值。

 - 对于序列$a$，维护递减的单调队列$mx$，使得`mx.front()`始终为最大值的下标。
 - 对于序列$b$，维护递减的单调队列$mn$，使得`mn.front()`始终为最小值的下标。

进而遍历每个数并依次入队、维护，设左端点为$j$，则当两个队列的对首相等，且下标不小于$j$时，累计答案，并向后枚举$j$。

[C++代码#2](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689D2.cpp)

## E. Mike and Geometry Problem

给出$n$个区间，求出任取$k$个区间的交集所含整数点的个数，$1\leq k\leq n\leq2\*10^5$，区间范围为$\pm10^9$.

实际上，若一个点被$p$个区间包含，那么这个点对答案的贡献就是$\left(^p_k\right)$，

因此若计算出被$i$个区间包含的点的个数$p[i]$，最终的答案就是$\sum^n_{i=k}\left(^i_k\right)\*p[i]$。

但是点坐标范围较大，直接扫描是不行的，需要对点进行离散，或直接将区间映射为起点与端点。

我们发现，对于相邻的一些点，若均被$i$个区间所包含，则可同时处理这些点所组成的区间，因此只需记录始末两点。

对于包含区间的数目，可以借助树状数组的思路，$cnt_i$表示在坐标为$i$处，区间数目的变化量。

预处理时，起点在$x$处则`++cnt[x]`，若末端点在$x$处则`--cnt[x]`。

从而在map上进行遍历，并更新每个点的区间数目，对相邻两点所组成的区间进行处理。
[C++代码](https://github.com/Lodour/ACM-ICPC/blob/master/codes/codeforces/689E.cpp)


