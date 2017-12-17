title: Markdown Format Test
date: 2015-12-07 17:07:00
categories:
  - 文档
tags:
  - Markdown
  - MathJax
---

You can ignore this for this page is to test basic markdown syntax and MathJax.

<!--more-->

# 标题1

## 标题2

### 标题3.1

### 标题3.2

### 标题3.3

#### 标题4

##### 标题5

###### 标题6
 
> 这是区块引用的第一段，
这是区块引用的第一段。
> 这是区块引用的第二段，
这是区块引用的第二段。

> 这是1级区块引用
  ```cpp
  #include <iostream>
  using namespace std;
  ```
> > 这是2级内嵌区块引用
> >
> > > 这是3级内嵌区块引用
> >
> > 这是2级内嵌区块引用
> 
> 这是1级区块引用

* 列表1
* 列表1
* 列表1

1.  列表2
    列表2
    列表2
2. 列表2
3. 列表2

这是行内代码`printf("%d\n", ans);`以及c++代码块：
``` cpp
#include <iostream>
using namesapce std;
int main(){
  int n;
  cin >> n;
  cout << n << endl;
  return 0;
}
```

这是python代码块
``` python
import requests
Req = requests.Session()
Url = "http://mangogao.com"
ReqIndex = Req.get(Url)
print ReqIndex.content
```

[这是到主页的链接](http://busyliving.me)
[这是一个有标题的链接](http://busyliving.me "BusyLiving")


*这是斜体*
**这是强调**
~~这是删除~~

![这是图片](/uploads/123.gif "Pic")

这是自动链接
<mangogao@qq.com>
<http://busyliving.me>

这是表格

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

这是分割线

---

# 下面是公式

上下标$(x+y)^{(x+y)^z}=(1+{\rm e}^{x})^{2x^w}$

左右都有上下标$\sideset{^1_2}{^3_4} \bigotimes$

分数和大括号$f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right)$

根号$\sqrt{3}$和$\sqrt[n]{3}$

省略号$f(x_1,x_2,\ldots,x_n) = x_1^1 + x_2^2 + \cdots + x_n^n$

矢量$\vec{a}\cdot\vec{b}=0$

积分$\int_0^{+\infty} (1+{\rm e}^x)^x {\rm d}x$

极限$\lim\limits_{n\rightarrow{\infty}}\frac{1}{n(n+1)}$

累加$\sum\limits_{i=1}^n\frac{1}{i^2}$

累乘$\prod\limits_{i=1}^n\frac{1}{i^2}$

希腊字母:$\alpha\beta\gamma\Gamma\delta\Delta\epsilon\varepsilon\zeta\eta\theta\Theta\vartheta\iota\kappa\lambda\Lambda\mu\nu\xi\Xi\pi\Pi\varpi\rho\varrho\sigma\Sigma\varsigma\tau\upsilon\Upsilon\phi\Phi\varphi\chi\psi\Psi\omega\Omega$

数学符号:$\pm\times\div\mid\cdot\circ\ast\bigodot\bigotimes\bigoplus\leq\geq\neq\approx\equiv\sum\prod\coprod$

集合负号:$\emptyset\in\notin\subset\supset\subseteq\supseteq\bigcap\bigcup\bigvee\bigwedge\biguplus\bigsqcup$

对数运算:$\log\lg\ln$

三角符号:$\bot\angle30^\circ\sin\cos\tan\cot\sec\csc$

微积分:$\prime\int\iint\iiint\iiiint\oint\lim\infty\nabla$

逻辑:$\because\therefore\forall\exists\not=\not>\not\subset$

戴帽:$\hat{y}\check{y}\breve{y}$

连线符号:$\overline{a+b+c+d} \underline{a+b+c+d} \overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}$

箭头:$\uparrow\downarrow\Uparrow\Downarrow\rightarrow\leftarrow\Rightarrow\Leftarrow\longrightarrow\longleftarrow\Longrightarrow\Longleftarrow$

字体:
${\rm Roman}\ {\it Italy}\ {\bf Black}\ {\cal Copper}\ {\sf DengXian}\ {\mit MathXieTi}\ {\tt TypeWriter}$

下面是矩阵:

普通矩阵：
$$
\begin{matrix}
1 & 2 & 3 \\\\
4 & 5 & 6 \\\\
7 & 8 & 9
\end{matrix} \tag{1}
$$

带括号的矩阵：()[]{}
$$
 \left\\{
 \begin{matrix}
   1 & 2 & 3 \\\\
   4 & 5 & 6 \\\\
   7 & 8 & 9
  \end{matrix}
  \right\\} \tag{2}
$$

带省略号的矩阵: $\cdots\ddots\vdots$
$$
\left[
\begin{matrix}
 1      & 2      & \cdots & 4      \\\\
 7      & 6      & \cdots & 5      \\\\
 \vdots & \vdots & \ddots & \vdots \\\\
 8      & 9      & \cdots & 0      \\\\
\end{matrix}
\right] \tag{3}
$$

带参数的矩阵：`cc|c`中的`|`是显示的分割符，`c`是居中对齐
$$ 
\left[
    \begin{array}{cc|cc}
      1 & 2 & 3 & a \\\\
      4 & 5 & 6 & a
    \end{array}
\right] \tag{4}
$$

最后是一个行间$\bigl(\begin{smallmatrix} a & b \\\\ c & d \end{smallmatrix} \bigr)$矩阵


$$
\begin{align}
\sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\\\
 & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\\\
 & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\\\
 & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\\\
 & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
$$

$$
f(n) =
\begin{cases}
n/2, & \text{if $n$ is even} \\\\
 3n+1, & \text{if $n$ is odd}
\end{cases}
$$

$$
\left.
\begin{array}{l}
\text{if $n$ is even:}&n/2\\\\
\text{if $n$ is odd:}&3n+1
\end{array}
\right\\}
 =f(n)
$$

$$
f(n) =
\begin{cases}
\frac{n}{2}, & \text{if $n$ is even} \\\\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$

$$
f(n) =
\begin{cases}
\frac{n}{2}, & \text{if $n$ is even} \\\\[2ex]
3n+1, & \text{if $n$ is odd}
\end{cases}
$$

$$
\begin{array}{c|lcr}
n & \text{左对齐} & \text{居中对齐} & \text{右对齐} \\\\
\hline
1 & 0.24 & 1 & 125 \\\\
2 & -1 & 189 & -8 \\\\
3 & -20 & 2000 & 1+10i
\end{array}
$$

$$
% outer vertical array of arrays 外层垂直表格
\begin{array}{c}
 % inner horizontal array of arrays 内层水平表格
\begin{array}{cc}
 % inner array of minimum values 内层"最小值"数组
\begin{array}{c|cccc}
\text{min} & 0 & 1 & 2 & 3 \\\\
\hline
 0 & 0 & 0 & 0 & 0 \\\\
 1 & 0 & 1 & 1 & 1 \\\\
 2 & 0 & 1 & 2 & 2 \\\\
 3 & 0 & 1 & 2 & 3
\end{array}
 &
 % inner array of maximum values 内层"最大值"数组
\begin{array}{c|cccc}
\text{max}&0&1&2&3 \\\\
\hline
 0 & 0 & 1 & 2 & 3 \\\\
 1 & 1 & 1 & 2 & 3 \\\\
 2 & 2 & 2 & 2 & 3 \\\\
 3 & 3 & 3 & 3 & 3
\end{array}
\end{array}
 % 内层第一行表格组结束
\\\\
 % inner array of delta values 内层第二行Delta值数组
\begin{array}{c|cccc}
\Delta&0&1&2&3 \\\\
\hline
 0 & 0 & 1 & 2 & 3 \\\\
 1 & 1 & 0 & 1 & 2 \\\\
 2 & 2 & 1 & 0 & 1 \\\\
 3 & 3 & 2 & 1 & 0
\end{array}
 % 内层第二行表格组结束
\end{array}
$$

$$
\left\\{
\begin{array}{c}
a_1x+b_1y+c_1z=d_1 \\\\
a_2x+b_2y+c_2z=d_2 \\\\
a_3x+b_3y+c_3z=d_3
\end{array}
\right.
$$

$$
\begin{cases}
a_1x+b_1y+c_1z=d_1 \\\\
a_2x+b_2y+c_2z=d_2 \\\\
a_3x+b_3y+c_3z=d_3
\end{cases}
$$

$$ 
x = a_0 + \cfrac{1^2}{a_1 + \cfrac{2^2}{a_2 + \cfrac{3^2}{a_3 + \cfrac{4^4}{a_4 + \cdots}}}}
$$

$$
x = a_0 + \frac{1^2}{a_1 + \frac{2^2}{a_2 + \frac{3^2}{a_3 + \frac{4^4}{a_4 + \cdots}}}}
$$


[Reference...](http://www.tuicool.com/articles/mEzqeaM)
