---
title: 生成函数简介
categories:
  - 算法
tags:
  - 生成函数
math: true
date: 2018-06-04 16:23:44
---

## 生成函数

生成函数(generating function)又称母函数，是处理组合数学问题的一大利器。

<!--more-->

### 普通型生成函数

#### Definition

对于一个数列 $f=<f\_0,f\_1,f\_2\\dots>$ ，构造形式幂级数
$$
F(x) = \\sum\_{i=0}^{\\infty}f\_ix^i
$$
qi称为数列$f$的普通型生成函数（ordinary generating function, 下简称OGF）。

一些常见的OGF有：

| 数列                      | OGF               |
| ----------------------- | ----------------- |
| $<1,0,0,\\dots>$         | $1$               |
| $<1,1,1,\\dots>$         | $\\frac1{1-x}$     |
| $<1,2,3,\\dots>$         | $\\frac1{(1-x)^2}$ |
| $<1,-1,1,-1,\\dots>$     | $\\frac1{1+x}$     |
| $<1,2,1,0,0,\\dots>$     | $(1+x)^2$         |
| $<1,4,6,4,1,0,0,\\dots>$ | $(1+x)^4$         |

根据OGF的定义，容易发现OGF的运算和数列运算之间的关系：
$$
\\begin{aligned}
\\alpha F(x)+\\beta G(x)&=\\sum\_n(\\alpha f\_n+\\beta g\_n)x^n\\\\
x^mF(x)&=\\sum\_n[n\\geq m]f\_{n-m}x^n\\\\
\\frac{F(x)-\\sum\_{i=0}^{m-1}f\_ix^i}{x^m}&=\\sum\_nf\_{n+m}x^n\\\\
F(cx)&=\\sum\_nc^nf\_nx^n\\\\
F^\\prime(x)&=\\sum\_n(n+1)f\_{n+1}x^n\\\\
\\int\_0^xF(t)\\mathrm{d}t&=\\sum\_n[n>0]\\frac{f\_{n-1}}nx^n\\\\
F(x)G(x)&=\\sum\_n\\left(\\sum\_{i=0}^nf\_ig\_{n-i}\\right)x^n
\\end{aligned}
$$
以上所有公式都是比较显然的，在此不证。

不过特别要注意最后一个式子，即OGF的乘积对应数列卷积。

通过上面的运算规则可以推出下面的式子。

| 数列                                       | OGF              |
| ---------------------------------------- | ---------------- |
| $<1,c,c^2,c^3,\\dots>$                    | $\\frac1{1-cx}$   |
| $<1,n,\\binom n2,\\binom n3,\\dots>$        | $(1+x)^n$        |
| $<1,n,\\binom{n+1}2,\\binom{n+1}3,\\dots>$  | $(1-x)^{-n}$     |
| $<0,1,\\frac12,\\frac13,\\dots>$            | $\\ln\\frac1{1-x}$ |
| $<0,1,-\\frac12,\\frac13,-\\frac14,\\dots>$  | $\\ln(1+x)$       |
| $<1,1,\\frac12,\\frac16,\\frac1{24},\\dots>$ | $e^x$            |

你会发现以上都是一些看上去很简单的东西。那么生成函数到底哪里厉害了呢？

#### Examples

注：这里只是简介...所以并没有太多的例子...

##### Example 1

$$f\_n=\\begin{cases}n&n\\leqslant1\\\\f\_{n-1}+f\_{n-2}&n>1\\end{cases}$$

简单的Fibonacci数列。

我们令 $F(x)$ 表示其OGF，则
$$
\\begin{aligned}
F(x)&=\\sum\_nf\_nx^n\\\\
&=\\sum\_n(f\_n+f\_{n-1}+[n=1])x^n\\\\
&=F(x)+xF(x)+x
\\end{aligned}
$$
从中我们解出$F(x)=\\frac{x}{1-x-x^2}$。然而这有什么用呢？

我们令$\\phi=\\frac{1+\\sqrt5}{2},\\hat\\phi=\\frac{1-\\sqrt5}2$，可以计算出$F(x)=\\frac{1}{\\sqrt5}\\left(\\frac1{1-\\phi x}-\\frac1{1-\\hat\\phi x}\\right)$

根据$\\frac1{1-cx}=\\sum\_nc^nx^n$，我们可以知道$f\_n=\\frac1{\\sqrt5}\\left(\\phi^n-\\hat\\phi^n\\right)$。就是这么简单。

##### Example 2

~~Warning!~~

许多线性递推的例子与上面的$F$相类似，于是我们直接来跳到一个复杂的例子来。

众所周知的Catalan数列的生成函数如何呢？

$$f\_n=\\begin{cases}1&n=0\\\\\\sum\_{i=0}^{n-1}f\_if\_{n-i-1}&n>0\\end{cases}$$

则
$$
\\begin{aligned}
F(x)&=\\sum\_n\\left([n=0]+\\sum\_{i=0}^{n-1}f\_if\_{n-i-1}\\right)x^n\\\\
&=1+x\\sum\_n\\left(\\sum\_{i=0}^{n-1}f\_if\_{n-1-i}\\right)x^{n-1}\\\\
&=1+xF^2(x)
\\end{aligned}
$$
亦即$xF^2-F+1=0$。代入二次方程求根公式，我们可以得到$F(x)=\\frac{1\\pm\\sqrt{1-4x}}{2x}$。

然而$\\pm$取哪一个呢？我们发现一个数列$f$的生成函数$F$一定满足$F(0)=f\_0$（某些情况下或许应该写成$\\lim\_{x\\to0}F(x)=f\_0$）。然而$f\_0=1$，这意味着$F(0)=1$。而如果我们令$\\pm$取$+$，那么$F(0)=\\frac20$是未定义的，这显然不是一个多项式应有的表现。

于是我们得到：Catalan数的生成函数是$\\frac{1-\\sqrt{1-4x}}{2x}$。这个生成函数令人有点难以置信，我们可以尝试展开它~~Warning!!!~~。

展开它的方式是$(x+y)^c=\\sum\_i\\binom cix^iy^{c-i}$在等式右端收敛（$\\left|\\frac xy\\right|<1\\lor c\\in \\mathbb N$）的情况下成立。然而生成函数是**形式**幂级数，这意味着我们不需要考虑收敛性。

于是

$$
\\sqrt{1-4x}=\\sum\_n\\binom{\\frac12}{n}(-4x)^n
$$

$$
\\because\\binom{\\frac12}n=\\frac{\\frac12(\\frac12-1)\\dots(\\frac12-n+1)}{n!}=\\frac{(-1)^{n-1}1\\times1\\times3\\times\\dots\\times(2n-3)}{2^nn!}=\\frac{(-1)^{n-1}(2n-2)!}{2^{2n-1}n!(n-1)!}
$$

$$
\\therefore\\sqrt{1-4x}=\\sum\_n\\binom{\\frac12}{n}(-4x)^n=-2\\sum\_n\\frac{(2n-2)!}{n!(n-1)!}x^n
$$

这意味着$$\\frac{1-\\sqrt{1-4x}}{2x}=\\sum\_n\\frac{(2n)!}{n!(n+1)!}x^n$$，正是Catalan数的通项公式。

更多例子请见[生成函数](/tags/生成函数)（可能有点困难...然而我的Blog里没几道简单题qaq...）。

或者[Wikipedia](https://en.wikipedia.org/wiki/Generating\_function#Examples\_of\_generating\_functions\_for\_simple\_sequences)

### 指数型生成函数

#### Definition

指数型生成函数（exponential generating function，简称EGF），其定义为

$$
F(x)=\\sum\_nf\_n\\frac{x^n}{n!}
$$

其意义体现在其乘法操作上：
$$
F(x)G(x)=\\sum\_n\\left(\\sum\_{i=0}^n\\binom nif\_ig\_{n-i}\\right)x^i
$$
多出来的$\\binom ni$告诉我们它适用于排列的计算（相对于OGF的普通卷积对应组合）。

EGF的运算法则：
$$
\\begin{aligned}
\\alpha F(x)+\\beta G(x)&=\\sum\_n(\\alpha f\_n+\\beta g\_n)\\frac{x^n}{n!}\\\\
x^mF(x)&=\\sum\_n[n\\geq m]n^{\\underline m}f\_{n-m}\\frac{x^n}{n!}\\\\
\\frac{F(x)-\\sum\_{i=0}^{m-1}f\_ix^i}{x^m}&=\\sum\_n\\frac1{(n+m)^{\\underline m}}f\_{n+m}\\frac{x^n}{n!}\\\\
F(cx)&=\\sum\_nc^nf\_n\\frac{x^n}{n!}\\\\
F^\\prime(x)&=\\sum\_nf\_{n+1}\\frac{x^n}{n!}\\\\
\\int\_0^xF(t)\\mathrm{d}t&=\\sum\_n[n>0]f\_{n-1}\\frac{x^n}{n!}\\\\
F(x)G(x)&=\\sum\_n\\left(\\sum\_{i=0}^n\\binom nif\_ig\_{n-i}\\right)\\frac{x^n}{n!}
\\end{aligned}
$$
同样有一些基本的EGF：

| 数列                | EGF      |
| ----------------- | -------- |
| $<1,1,1,\\dots>$   | $e^x$    |
| $<0,1,2,\\dots>$   | $xe^x$   |
| $<1,c,c^2,\\dots>$ | $e^{cx}$ |
| $\\dots$           | $\\dots$  |

好吧你只需要注意到$<f\_n>$的EGF实际上就是$<\\frac{f\_n}{n!}>$的OGF，就能找到不少例子。

那么EGF又有什么作用呢？

#### Examples

##### Example 1

贝尔数$\\omega\_n$是把大小为$n$的子集分割成若干非空不相交子集的方案数。

通过枚举第一个元素所在的子集大小$i$，我们可以得到

$$\\omega\_n=[n=0]+\\sum\_{i=1}^n\\binom{n-1}{i-1}\\omega\_{n-i}$$

于是其EGF满足

$$F(x)=1+\\int\_0^xe^tF(t)\\mathrm{d}t$$

两边求导
$$
\\begin{aligned}
F^\\prime(x)&=e^xF(x)\\\\
\\frac{\\mathrm{d}F}{\\mathrm{d}x}&=e^xF(x)\\\\
\\frac{\\mathrm{d}F}F&=e^x\\mathrm{d}x\\\\
\\int\\frac{\\mathrm{d}F}F&=\\int e^x\\mathrm{d}x\\\\
\\ln F&=e^x+C\\\\
F&=e^{e^x+C}\\\\
\\end{aligned}
$$
把$F(0)=\\omega\_0=1$代入即得$F(x)=e^{e^x-1}$。这个多项式的系数可以利用$e^g=\\sum\_n\\frac{g^n}{n!}$计算。

$F(x)=e^{e^x-1}$也可以这样理解：$g(x)=e^x-1$的$n$次项系数是“n个数组成一个非空集合”的方案数（显然），而$g^c(x)$的$n$次项系数（由于EGF对应排列）就是“n个数划分成c个非空集合”的方案数。而由于这$c$个集合是不可区分的，所以要乘上$\\frac1{c!}$。于是我们得到$F(x)=\\sum\_n\\frac{g^n(x)}{n!}=e^{g(x)}=e^{e^x-1}$

~~留坑待填~~

~~PS:\_rqy太懒了以至于他不想写了~~
PS2: 已经坑了。
