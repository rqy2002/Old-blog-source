---
title: 类型论学习笔记 (0) 入门科普
categories:
  - Type_Theory
mathjax: true
date: 2019-10-13 13:23:18
---

大概从这篇 Blog 开始会写一系列学习笔记，以 Type Theory 为中心写一些学习时的问题和得到的解答，也可能会作为半科普or入门读物。术语的中文翻译基本上都是从 Software-Foundation 那里看来的。

在此感谢 [千里冰封](https://ice1000.org) 耐心解答我的问题。

作为第 0 篇 Blog，这里是科普文章。求 dalao 轻喷。

<!--more-->

---

**类型论**和集合论类似，是一类公理体系。其研究对象是**项** （*term*） 以及它们的**类型** （*type*）。

为了理解什么是类型论，我们造一个没有用的玩具语言。

## 简单的例子

### 文字叙述

这种语言只有 ${\bf Bool}$ 和 ${\bf Int}$ 两种类型，而项可以由数字、${\rm True}$、${\rm False}$、${\rm if}\ b\ {\rm then}\ u \ {\rm else}\ v$ 以及加减来表示（当然补充上乘除也可以，为了方便这里只使用加减）。

我们有一些推导规则：

1. 任何一个整数属于 ${\bf Int}$ 类型；
2. ${\rm True}$ 和 ${\rm False}$ 属于 ${\bf Bool}$ 类型；
3. 如果 $a$, $b$ 都属于 ${\bf Int}$ 类型，那么 $a+b$ 属于 ${\bf Int}$ 类型，$a-b$ 属于 ${\bf Int}$ 类型。
4. 如果 $b$ 属于 ${\bf Bool}$ 类型，而 $u,v$ 都属于 $T$ 类型（注意 $T$ 是不固定的），那么 ${\rm if}\ b\ {\rm then}\ u\ {\rm else}\ v$ 属于 $T$ 类型。

比如，$14$ 属于 ${\bf Int}$，$9$ 属于 ${\bf Int}$，因此 $14+9$ 属于 ${\bf Int}$，这是由第 3 条规则推导而出的。

注意到这个语言中会有“不合法”的项，比如 ${\rm if}\ 13\ {\rm then}\ 5\ {\rm else}\ 4$，显然它类型不正确。我们称所有具有某种类型的项为 **良类型** 的（*well-typed*）。

这个语言没有什么用处，但是可以让我们认识到类型论中的一些记法。

### 形式化叙述

以文字来叙述规则在系统更复杂的时候很容易出问题，所以以形式化的语言来叙述是有必要的。

用 $a:T$ 表示项 $a$ 属于类型 $T$，$\vdash a:T$ 表示“我们可以推断出 $a:T$”的事实。$\vdash$ 叫做十字转门（*turnstile*，翻译好像是直译），之后我们会在它左边添加“上下文”，但是现在它左边总是空的。

我们可以把推导规则写成下面的形式：
$$
\cfrac{v{\rm\ is\ a\ number}}{\vdash v:{\bf Int}}{\rm\tiny(NUM)}
\qquad\cfrac{}{\vdash {\rm True}:{\bf Bool}}{\rm\tiny(TRUE)}
\qquad\cfrac{}{\vdash {\rm False}:{\bf Bool}}{\rm\tiny(FALSE)}
$$
$$
\cfrac{\vdash a:{\bf Int}\quad\vdash b:{\bf Int}}{\vdash a+b:{\bf Int}}{\rm\tiny(PLUS)}
\qquad\cfrac{\vdash a:{\bf Int}\quad\vdash b:{\bf Int}}{\vdash a-b:{\bf Int}}{\rm\tiny(MINUS)}
\quad\cfrac{\vdash b:{\bf Bool}\quad\vdash u:T\quad\vdash v:T}{\vdash ({\rm if}\ b\ {\rm then}\ u\ {\rm else}\ v):T}{\rm\tiny(IF)}
$$

这里我们用一条横线表示从上至下的推导，横线上方会有若干个前提条件（也可能没有，比如这里的 (TRUE) 规则），而横线下方会有一个结论。横线右边的标记表示这条规则的名字。这样就表示一条**推导规则**（*inference rule*）。

这样写的好处在于，我们可以通过一条一条横线叠加来证明一条结论。比如我们要证明 $({\rm if\ True\ then}\ (14+5)\ {\rm else}\ 3):{\bf Int}$，就可以这样写：
$$
\cfrac{
  \cfrac{}{\vdash {\rm True}:{\bf Bool}}{\rm\tiny(TRUE)}
  \quad\cfrac{
    \cfrac{14{\rm\ is\ a\ number}}{\vdash 14:{\bf Int}}{\rm\tiny(NUM)}
    \quad\cfrac{5{\rm\ is\ a\ number}}{\vdash 5:{\bf Int}}{\rm\tiny(NUM)}
  }{\vdash (14+5):{\bf Int}}{\rm\tiny(PLUS)}
  \quad\cfrac{3{\rm\ is\ a\ number}}{\vdash 3:{\bf Int}}{\rm\tiny(NUM)}
}{\vdash ({\rm if\ True\ then}\ (14+5)\ {\rm else}\ 3):{\bf Int}}{\rm\tiny(IF)}
$$
当然这样逐字的写出证明来很无聊也很麻烦。

### 归约

项的**归约**（*reduction*）是一系列规则，用来描述一个项如何“化简”。比如这里我们可以定义一些规则：
$$
\begin{aligned}
{\rm if\ True\ then}\ u\ {\rm else}\ v&\twoheadrightarrow u&{\rm\tiny(IF\_TRUE)}\\
{\rm if\ False\ then}\ u\ {\rm else}\ v&\twoheadrightarrow v&{\rm\tiny(IF\_FALSE)}\\
n_1+n_2&\twoheadrightarrow {\rm the\ number }\ (n_1+n_2)&{\rm\tiny(PLUS\_NUM)}\\
n_1-n_2&\twoheadrightarrow {\rm the\ number }\ (n_1-n_2)&{\rm\tiny(MINUS\_NUM)}\\
\end{aligned}
$$

（其中 $n_1,n_2$ 是两个整数）

实际上我们可以对一个项的任何子项做这样的归约，读者可以自行思考具体定义（无非就是 $a\twoheadrightarrow a'$ 时 $a+b\twoheadrightarrow a'+b$ 这种规则）。

如果两个项 $a,b$ 可以归约到同一个项，那么记它们两个相等，记做 $a=b$。为了与此区分，两个项完全相同，即**句法等价**（*syntactically equivalent*），也就是不需要归约地相同时记做 $a\equiv b$。

这里没有给出归约的严谨定义，具体可以去看相关书籍。

需要注意的是，一个项归约之后是另一个项，当 a 规约为 b 的时候*不能直接*从 $\vdash b:T$ 推出 $\vdash a:T$ 或者反过来。但是在我们的玩具语言中，可以证明若 $\vdash a$ 且 $T;a\twoheadrightarrow b$，则 $\vdash b:T$。反过来是**不**可以的，因为 if-else 里面的 u 和 v 必须是同一个类型，而归约之后可能丢掉其中一个。

这并不是公理，而是从 reduction rule 和 inference rule 得到的。

### 上下文

注意这个语言是很无趣的，因为一个 well-typed 的项总可以归约到一个数或者 True 或者 False。

但是当我们在语言中添加“变量”的时候，它就不同了。比如我们可以写 ${\rm if}\ b\ {\rm then\ False\ else\ True}$，可以发现这其实就是 ${\rm not}\ b$。因此我们补充语言的定义：一个项也可以包含**变量**（*variable*）。

但是这样问题来了：我们没有任何方式判定一个变量应当是什么类型。

那么我们就可以有一个上下文的概念：我们钦定一个（或一些）变量的类型，把它写在 $\vdash$ 左侧。这些钦定的条件叫做**上下文**或**环境**（*context*），一般用 $\Gamma,\Delta$ 这些大写希腊字母表示，它实际上是一系列变量的类型限制，比如 $\Gamma::=x:{\bf Int},b:{\bf Bool}$ 这样。当然既然类型都可以钦定了，我们也自然地想到可以钦定一些不确定的类型，比如 $x:A,y:B$ 这样。

于是记 $\Gamma\vdash a:T$ 表示“在 $\Gamma$ 这个上下文下，$a$ 的类型是 $T$”。比如我们可以自然的认为应该有
$$
b:{\bf Bool}\vdash({\rm if}\ b\ {\rm then\ False\ else\ True}):{\bf Bool}
$$
这样的话类型推导规则需要修改如下：
$$
\cfrac{(x:A)\in \Gamma}{\Gamma\vdash x:A}{\rm\tiny(VAR)}
\qquad\cfrac{v{\rm\ is\ a\ number}}{\Gamma\vdash v:{\bf Int}}{\rm\tiny(NUM)}
\qquad\cfrac{}{\Gamma\vdash {\rm True}:{\bf Bool}}{\rm\tiny(TRUE)}
\qquad\cfrac{}{\Gamma\vdash {\rm False}:{\bf Bool}}{\rm\tiny(FALSE)}
$$

$$
\cfrac{\Gamma\vdash a:{\bf Int}\quad\Gamma\vdash b:{\bf Int}}{\Gamma\vdash a+b:{\bf Int}}{\rm\tiny(PLUS)}
\qquad\cfrac{\Gamma\vdash a:{\bf Int}\quad\Gamma\vdash b:{\bf Int}}{\Gamma\vdash a-b:{\bf Int}}{\rm\tiny(MINUS)}
\quad\cfrac{\Gamma\vdash b:{\bf Bool}\quad\Gamma\vdash u:T\quad\Gamma\vdash v:T}{\Gamma\vdash ({\rm if}\ b\ {\rm then}\ u\ {\rm else}\ v):T}{\rm\tiny(IF)}
$$

简单的说，我们要求前提条件和推导出的结论中的上下文都是相同的（以后我们会遇到不同的情况），并且加了一条规则：当我们钦定 $x:A$ 的时候我们就可以得到 $x:A$。这样我们就可以推导出
$$
{b:{\bf Bool}, u:{\bf Int}\vdash ({\rm if}\ b\ {\rm then}\ (u+13)\ {\rm else}\ 7):{\bf Int}}\\
{b:{\bf Bool}, u:T\vdash ({\rm if}\ b\ {\rm then}\ u\ {\rm else}\ u):T}
$$
这些~~trivial的~~事实。

## $\lambda$ 演算及 $\lambda_\to$ 类型系统

$\lambda$ 演算是许多类型论的基础。我们先叙述其定义，再叙述其归约规则，之后给出其上的一个类型系统，即$\lambda_\to$ 系统。

### 定义

$\lambda$ 演算中一个项的定义为 $t::= x \mid t_1 t_2\mid\lambda x.t$。也就是说，每个项或者是一个变量，或者是两个项拼起来，或者是形如 $\lambda x.t$ 的形式，其中 $x$ 是一个变量而 $t$ 是另一个项。当然是可以加括号的。

$t_1t_2$ 这种形式称为**应用**（*application*）。想象 $t_1$ 表示一个函数而 $t_2$ 表示函数参数，那么 $t_1t_2$ 就表示函数应用。这东西是左结合的，也就是说当我们写下 $t_1t_2t_3\dots t_n$的时候，我们实际上在说 $((\dots(t_1t_2)t_3)\dots)t_n$。

$\lambda x.t$ 称为**抽象**（*abstraction*）。直观的说，它表示一个函数，当传入一个参数 $N$ 的时候，得到的是 $t[x:=N]$（这个记号大概是说是把 $t$ 里面的 $x$ 都换成 $N$）。这东西优先级比 application 高，也就是说 $\lambda x. t_1t_2$ 表示 $\lambda x. (t_1t_2)$ 而不是 $(\lambda x.t_1)t_2$。

比方说，$\lambda x.\lambda y. x$（表示 $\lambda x.(\lambda y.x)$），表示“传入一个参数 $x_0$ 之后返回一个函数 $f=\lambda y. x_0$，这个函数 $f$ 传入任何一个参数都会返回 $x_0$”，也就是说$(\lambda x.\lambda y.x)x_0y_0=x_0$。

我们以后不区分这种说法和“传入两个参数 $x_0,y_0$ 之后会返回 $x_0$”的说法（这称为柯里化 curry （反义词是 uncurry ））。

当然这只是直观的意义，具体的意义要以归约规则决定。

### 自由的/绑定了的变量

在介绍归约规则之前，我们先来看**自由变量**（*free variable*）和**绑定了的变量**（*bounded variable*）的概念。

一个项里的某个变量 $x$ 称为自由的当且仅当它没有匹配上某个抽象。比如说 $y (\lambda x. x)$ 这个项里 $y$ 是 free 的而 $x$ 是 bounded 的。同一个名字的变量也可以既有自由的也有约束的，比如 ${\color{red}x}(\lambda x. {\color{green}x})$ 里红色的 $x$ 是 free 的而绿色的 $x$ 是 bounded 的。注意 $\lambda$ 后面那个位置不算 $x$ 出现了，它在这里只是一个记号。

具体地说，单个一个 $x$ 的项中 $x$ 是 free 的；$t_1t_2$ 不会改变其中每个变量是否 free；$\lambda x. t$ 则会把 $t$ 中所有的 $x$ 都 bound 起来，不会影响其他变量。

做一个类比，自由变量就是全局变量而绑定了的变量就是局部变量。

之前我们说 $M[x:=N]$ 是说把 $M$ 里的 $x$ 都换成 $N$，现在我们可以表达它的大致准确的意思：我们把所有自由出现的 $x$ 都替换成 $N$，比如 $(y(\lambda x. x)x)[x:=(zw)]$ 就是 $y(\lambda x.x)(zw)$。

对于一个 $\lambda x. M$，我们可以把 $M$ 中所有的 free 的 $x$ 一起改个名字。比如说 $\lambda x. xy$ 可以改成 $\lambda a.ay$。这就是接下来要说的 $\alpha$-conversion。

### $\alpha$-conversion

如上所述，我们可以给局部变量改一个名字，比如 $\lambda x.xy$ 改成 $\lambda a.ay$，或者 $\lambda x.(\lambda x. x)$ 变成$\lambda y.(\lambda x.x)$（因为里面的 $x$ 不是 free 的，所以不需要改）。但是也不是随便改，比如我们不能把 $\lambda x.xy$ 改成 $\lambda y.yy$。

所以说我们的要求大概是：在 $\lambda x.M$ 中，如果 $M$ 中所有 $y$ 都是 bounded 的（或者根本不出现），那么我们可以把它变成 $\lambda y. (M[x:=y])$。这样的转化叫做 $\alpha$-conversion。一般我们认为通过 $\alpha$-conversion 能够互相转化的项也是相同的（句法等价的，即 $\equiv$，而不是可以归约到相同项意义下的相等）。

$\alpha$-conversion 的必要性在于，我们在计算 $M[x:=N]$ 的时候有时候需要对 $M$ 中别的变量进行更名，比如 $(\lambda y. xy)[x:=(yz)]$ 应该变成 $(\lambda a.(yz)a)$ 而不能变成 $(\lambda y. (yz)y)$。后者会使得本应该 free 出现的 y 被错误地 bound 掉了。

另外还有一个称为 $\eta$-conversion 的东西， 大概是说 $x$ 不在 $M$ 中 free 出现的时候， $\lambda x. Mx\equiv M$。这条一般来说是不必要的，但是加上一般也不会出问题。

### 归约规则

归约规则称为 $\beta$-reduction，它也很简单：$(\lambda x.M)N\twoheadrightarrow M[x:=N]$。有时候也会记作 $\twoheadrightarrow_\beta$。

简单的说，就是一个函数应用。~~等下，这个规则怎么这么简单~~

这种东西是有一大堆（看上去显然的）性质的，比如如果 $M$ 能分别归约到 $N_1$ 和 $N_2$，那么后两者也一定能归约到完全相同的项。

类似的，如果两个项 $M,N$ 可以归约到完全相同的项，那么就称它们是相等的，记做 $M=N$ 或者 $M=_\beta N$。

### $\lambda_\to$ 系统

#### 定义

这是一个很简单但是性质却不是那么简单的类型系统，不同于前面那个仅仅只能用来举例子的玩具语言，有什么性质几乎是一目了然的并且证明也很 trivial。

类型也很简单：一个类型定义为 $T::=A\mid T_1\to T_2$，也就是说一个类型要么是一个 primal 的名字，要么是 $T_1\to T_2$ 的形式。

$A\to B$ 的类型称为**箭头类型**（*arrow type*），直观上表示 $A$ 到 $B$ 的函数。这个是右结合的，也就是说 $A\to B\to C$ 表示 $A\to(B\to C)$。

#### 推导规则

推导规则其实很简单：单独变量的类型是需要钦定的；若 $M:A\to B,N:A$ 则 $(MN):B$（也就是函数应用）。$\lambda x.M$ 的推导则是这样的：如果我们额外钦定 $x:A$ 的时候 $M:B$ ，那么就可以得到 $(\lambda x. M):(A\to B)$。
$$
\cfrac{(x:A)\in\Gamma}{\Gamma\vdash x:A}{\rm\tiny(VAR)}
\qquad\cfrac{\Gamma\vdash M:(A\to B)\quad\Gamma\vdash N:A}{\Gamma\vdash (MN):B}{\rm\tiny(APP)}
\qquad\cfrac{\Gamma,x:A\vdash M:B}{\Gamma\vdash (\lambda x.M):(A\to B)}{\rm\tiny(ABS)}
$$
(ABS) 规则中 $\Gamma, x:A$ 是指扩充上下文，补充 $x:A$，如果 $\Gamma$ 中本来就有 $x:C$ 那么就把那个东西去掉（意会一下，里面的局部变量屏蔽了外面的局部变量）。

三条 Inference Rule 就可以得到一个有用的类型系统还是很优美的。比如我们可以这样推导：
$$
\cfrac{\cfrac{x:A,y:B\vdash x:A}{x:A\vdash(\lambda y.x):(B\to A)}}{\vdash (\lambda x.\lambda y.x):(A\to B\to A)}
$$
这两步都使用了 (ABS) 规则。一种简便的写法是不写 Context 和 $\vdash$ 符号，在对某个变量进行抽象的时候把它划掉。比如
$$
\cfrac{\cfrac{\cfrac{\{x:A\}^2\quad\{y:B\}^1}{x:A}}{(\lambda y.x):(B\to A)}\small1}{(\lambda x.\lambda y.x):(A\to B\to A)}\small2
$$
由于没法打删除线，请自行想象把花括号改成删除线（

就是说推导到 1 那一步的时候把 $y:B$ 划掉（因为这一步之后它就不在上下文里了），2 那一步的时候把 $x:A$ 划掉。

### 一些性质

$\lambda_\to$ 系统中有一些性质（定理），比如：

1. 若 $\Gamma\vdash M:A$，那么 $M$ 中的自由变量都在 $\Gamma$ 里出现，特别的，如果 $\vdash M:A$，那么 $M$ 中不含自由变量；
2. 如果 $\Gamma \vdash M:A, \Gamma\subseteq\Gamma_1$，则 $\Gamma_1\vdash M:A$；
3. 若 $\Gamma\vdash M:A,M\twoheadrightarrow_\beta M'$，则 $\Gamma\vdash M':A$ （反过来不成立，如 $(\lambda y.\lambda x.x)(yy)$ 没有类型）；
4. 若 $\Gamma\vdash M:A,\Gamma\vdash M':A',M=_\beta M'$，则 $A\equiv A'$。

当然还有好多，这里不再赘述。

## 柯里-霍华德同构

**柯里-霍华德同构**（*Curry-Howard  Isomorphism*）建立了类型系统和形式逻辑系统之间的对应（类型即命题，程序即证明）。

$\lambda_\to$ 类型系统就对应于命题逻辑。考虑每个类型 $A$ 对应一个命题（也用 $A$ 表示），$A\to B$ 的类型对应 “$A$ 蕴含 $B$” 这个命题。一个项对应命题的一个证明。

这样的话，由于 $A$ 蕴含 $B$ 等价于“只要有一个 $A$ 的证明，就可以构造一个 $B$ 的证明”，所以这和“给出一个 $A$ 类型的项，应用这个函数就可以得到一个 $B$ 类型的项”是等价的。

举个例子，命题逻辑一种仅使用蕴含来表示的公理系统中某两条公理分别是：
$$
A\to (B\to A);\\
(A\to(B\to C))\to((A\to B)\to(A\to C))
$$
对应到类型上，就可以用如下的项给出：
$$
\lambda x.\lambda y.x\\
\lambda x.\lambda y.\lambda z.xz(yz)
$$
分离规则
$$
\cfrac{\vdash A\to B\quad\vdash A}{\vdash B}
$$
也正是 application。

上下文也可以对应起来：类型系统中一个上下文 $\Gamma$，对应到逻辑系统中就是说 $\Gamma$ 中所有类型作为前提条件。(ABS) 规则也对应了 $A\vdash B$ 则 $\vdash A\to B$ 的定理。

当我们之后扩展类型系统的时候，就可以用它来对应更复杂的逻辑系统，如高阶命题逻辑、谓词逻辑等。

这里还有一个小问题：命题逻辑中 $\lnot A$ 应该对应什么？

在 $\lambda_\to$ 中，我们可以扩展类型系统，添加 $\bot$ 类型。它表示“空类型”，即不存在这种类型的项，对应逻辑上的永假命题。并且可以加一条规则：$\cfrac{\Gamma\vdash x:\bot}{\Gamma\vdash x:A}$。简单的说，如果我们得到了一个空类型的值，就可以得到任意类型的值。对应到逻辑上就是说，如果我们得到了矛盾那就可以导出任何结论。

那么就可以定义 $\lnot A=A\to\bot$。

这样又出现了新的问题：我们不能得到这样的项：$\lnot(\lnot A)\to A$。没有这样的项，就不能进行反证（反证法本质就是证明原命题的否定是假，从而证明原命题为真）。于是添加 $\bot$ 之后 $\lambda_\to$ 事实上对应**直觉主义逻辑**，或者称为**构造主义逻辑**，因为我们本质上只能构造一个证明，而不能通过排中律（$A\lor\lnot A$）、二次互反律（$\lnot(\lnot A)\to A$）来做证明，除非我们把这种类型的项一直放在 Context 里（即，设定为公理）。

## 总结

这一篇 Blog 大致写了类型系统是什么样子，提供一个入门。但是很多东西是没有完全严格定义的，如果有想了解的可以自己去查资料、找书看。我是看的 *Lambda Calculi with Types, Henk Barendregt* 入门。

简要解释了 Curry-Howard 同构，它将类型系统与逻辑系统联系起来从而使得*形式化验证*（即借助计算机来证明定理，可以避免证明错误、并且某些情况下可以通过自动化的步骤来证明一些过程简化证明）成为可能，因为它把寻找一个证明转化成了写一个给定类型的程序。

之后的博客大概会先介绍 Dependent Type、 Lambda Cube 之类的数学基础，然后随着我尝试自己实现一种 DT 的语言来写这之间遇到的问题及解决方案的笔记。
