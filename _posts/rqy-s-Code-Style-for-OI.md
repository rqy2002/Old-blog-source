---
title: _rqy's Code Style for OI
tags:
  - 代码规范
top: true
mathjax: true
date: 2018-03-14 10:27:47
---

本文介绍\_rqy的OI中的代码规范。其来源主要为\_rqy的长期积累及参考Google代码规范、Menci的规范。

<!--more-->

*可能会update。*

> Inspired by [Menci's Code Sytle for OI](https://oi.men.ci/code-style-oi/)

## 概述

`#include`语句**必须**置于整个程序的开头。

不应`using namespace foo;`。若有必要可以`using foo::bar;`。

单行字符数**必须**不超过80。

## 预编译

`#include`的多个库顺序可有以下两种：

1. C++标准库在前，之后是C标准库，再后为其它（如交互库等）。（工程代码中，本`cpp`所对应的`.h`文件应置于开头。）
2. （仅适用于OI）按字典序依次排列。

可以使用`#include <foo>`时**绝不**使用`#include "foo"`。

如果有多层嵌套`#if #endif`,`#endif`后应有对应的注释标识出与其对应的`#if`。

尽量**不要**适用`#define`而使用`const`, `typedef`, `inline`。

所有预编译命令**不应**缩进（见下）。

## 缩进

每个代码块采用2空格缩进。

## 空格及换行

大括号**不**换行。

需要加空格的地方：

1. 二元运算符（包括赋值运算符）两侧（`,`运算符例外，见下）；
2. `,`及`;`的右边（如果其不处于行尾）；
3. `if`, `for`等控制流关键字与其后的左括号之间 ；
4. `do-while`中的`while`、`if-else`中的`else`与其前面的右大括号之间；
5. 所有左大括号的左侧（根据不换行的策略，左大括号不应处于行首）；
6. `?` `:`的两侧（包括构造函数初始化列表中的`:`）；
7. 类型中`*`,`&`的左侧（如：`const int &a`, `int A(int *&a)`。）；
8. 花括号与其内部语句/数组初始化列表之间（如果在同一行）；
9. 常成员函数的`const`两侧。

**一定不**能加空格的地方：

1. 小括号及中括号与其内部的表达式/参数列表之间；
2. 函数名与左括号之间（包括声明/定义/使用）；
3. 单目运算符（`!`,`-`,`*`,`&`,`~`）之后（或自增自减运算符与其操作数之间）；
4. `,`及`;`的左侧；
5. 类型中`*`,`&`的右侧；
6. `.`,`->`,`::`的两侧；
7. `operator`与所要重载的运算符之间（运算符与参数列表之间，根据第2条，也不应空格）。

若表达式过长内部可以换行，运算符处于**行首**（而非行尾）；缩进以使表达式对齐为准；换行的优先级较高的子表达式也应加括号以避免误读。

参数列表/初始化列表过长时内部也可换行，逗号处于**行尾**；缩四空格。

极短的函数可以写到一行（但绝不能超过80字符）。

例子：

```cpp
struct AVeryVeryVeryVeryVeryVeryVeryVeryVeryLongStruct{
  int aVeryVeryVeryVeryVeryVeryVeryVeryVeryVeryLongVariable, d;
  AVeryVeryVeryVeryVeryVeryVeryVeryVeryLongStruct(int a, int b, int c, int d)
      : aVeryVeryVeryVeryVeryVeryVeryVeryVeryVeryLongVariable(a) {
    this->d = b + c * d;
  }
};

inline int min(int x, int y) { return x < y ? x : y; }
int gcd(int x, int y) { return y ? gcd(y, x % y) : x; }

int main() {
  int thisVariableIsToBeLong = 2, thisVariableIsToBeLongerAndLonger = 23;
  AVeryVeryVeryVeryVeryVeryVeryVeryVeryLongStruct s(
      thisVariableIsToBeLong, thisVariableIsToBeLong,
      thisVariableIsToBeLongerAndLonger,
      thisVariableIsToBeLongerAndLonger
  );
  printf("%d\n", s.d);
}
```



## 空行

所有`#include <foobar>`与`using foo::bar;`之间不应空行，之后应空一行。

一系列常量定义的上下应有空行。

函数/结构体定义两侧应有空行（一系列短小到可以写到一行的函数，如`min`,`max`，之间可以不空行）。

一系列全局变量定义的上下应有空行。

语句之间可根据其意义酌情空行。

任何位置**不能**出现连续的两个（或以上）空行。

## 函数定义

`main`函数返回值必须为`int`,`return 0`**不可**忽略；

类/结构体传参在大多数情况下**不应**传值（除非难以避免地产生拷贝，或一些特殊要求），而应传引用。

极其简短的函数可以写作一行（但**绝不能**超过80字符），此时花括号内部应有空格（空函数体`{}`除外）。

单个函数的长度不应过长（例如超过100行）。

## 命名规则

一般情况下应采用驼峰命名法，变量开头小写，函数/类/结构体开头大写。

结构体/类成员函数，可以小写开头。

特例：

1. `main`函数；
2. 变量可以以一个小写字母命名；
3. 全局数组名可使用1个大写字母+0~2个数字命名，如`A`, `T1`,`F01`；
4. 模板。如`readInt`, `pow_mod`;
5. 采用对应算法缩写，如`KMP`, `CRT`, `NTT`,`CDQ`；
6. 简短的inline函数，如`min`, `upd`(用作数据结构中的update操作);
7. 常量可以大写字母命名，如`N`, `M`；
8. 临时变量可以以下划线开头。

## Example Code

```cpp
#include <algorithm>
#include <cctype>
#include <cstdio>

typedef long long LL;

inline int readInt() {
  int ans = 0, c;
  while (!isdigit(c = getchar()));
  do ans = ans * 10 + c - '0';
  while (isdigit(c = getchar()));
  return ans;
}

const int mod = 998244353;
const int g = 3;
const int N = 200050;

inline LL pow_mod(LL x, int p) {
  LL ans = 1;
  for ((p += mod - 1) %= (mod - 1); p; p >>= 1, (x *= x) %= mod)
    if (p & 1) (ans *= x) %= mod;
  return ans;
}

LL inv[N];
int n;

void NTT(LL *A, int len, int opt) {
  for (int i = 1, j = 0; i < len; ++i) {
    int k = len;
    while (~j & k) j ^= (k >>= 1);
    if (i < j) std::swap(A[i], A[j]);
  }

  for (int h = 2; h <= len; h <<= 1) {
    LL wn = pow_mod(g, (mod - 1) / h * opt);
    for (int j = 0; j < len; j += h) {
      LL w = 1;
      for (int i = j; i < j + (h >> 1); ++i) {
        LL _tmp1 = A[i], _tmp2 = A[i + (h >> 1)] * w;
        A[i] = (_tmp1 + _tmp2) % mod;
        A[i + (h >> 1)] = (_tmp1 - _tmp2) % mod;
        (w *= wn) %= mod;
      }
    }
  }

  if (opt == -1)
    for (int i = 0; i < len; ++i)
      (A[i] *= -(mod - 1) / len) %= mod;
}

LL F[N], G[N];
LL T1[N * 4], T2[N * 4];

void Conv(LL *A, int n, LL *B, int m) {
  int len = 1;
  while (len <= n + m) len <<= 1;
  for (int i = 0; i < len; ++i)
    T1[i] = (i < n ? A[i] : 0);
  for (int i = 0; i < len; ++i)
    T2[i] = (i < m ? B[i] : 0);

  NTT(T1, len, 1);
  NTT(T2, len, 1); 
  for (int i = 0; i < len; ++i)
    (T1[i] *= T2[i]) %= mod;
  NTT(T1, len, -1);
}

void Solve(int l, int r) {
  if (l == r - 1) {
    F[l] = (l == 0 ? 1 : F[l] * inv[l] % mod);
    return;
  }

  int mid = (l + r) >> 1;
  Solve(l, mid);
  Conv(F + l, mid - l, G, r - l);
  for (int i = mid; i < r; ++i)
    (F[i] += T1[i - l]) %= mod;
  Solve(mid, r);
}

int main() {
  n = readInt();

  inv[1] = 1;
  for (int i = 2; i <= n; ++i)
    inv[i] = -(mod / i) * inv[mod % i] % mod;

  for (int i = 1; i <= n; ++i) {
    scanf("%lld", &G[i]);
    (G[i] *= i) %= mod;
  }

  Solve(0, n + 1);
  for (int i = 1; i <= n; ++i)
    printf("%lld\n", (F[i] + mod) % mod);
  return 0;
}

```

注：此为多项式$\exp$模板。
