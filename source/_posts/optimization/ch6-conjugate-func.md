---
title: 凸优化笔记 6：共轭函数 Conjugate Function
date: 2020-03-04 23:49:41
tags:
  - 共轭函数
categories:
  - Convex Optimization
mathjax: true
---

## 1. 共轭函数

### 1.1 定义

一个函数 $f$ 的**共轭函数(conjugate function)** 定义为
$$
f^*(y)=\sup_{x\in\text{dom}f}(y^Tx-f(x))
$$
![conjugate function](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/6-conjugate.PNG)

<!--more-->

$f^*$ 是凸函数，证明也很简单，可以看成是一系列关于 $y$ 的凸函数取上确界。

**Remarks**：实际上共轭函数与前面讲的一系列支撑超平面包围 $f$ 很类似，通过 $y$ 取不同的值，也就获得了不同斜率的支撑超平面，最后把 $f$ 包围起来，就好像是得到了 $\text{epi }f$ 的一个闭包，如下图所示

![conjugate function](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/6-conjugate2.PNG)

### 1.2 性质

关于共轭函数有以下性质

1. 若 $f$ 为凸的且是闭的($\text{epi }f$ 为闭集)，则 $f^{**}=f$ (可以联系上面提到一系列支撑超平面)
2. (Fenchel's inequality) $f(x)+f^*(y)\ge x^Ty$，这可以类比均值不等式
3. (Legendre transform)如果 $f\in C^1$，且为凸的、闭的，设 $x^*=\arg\max\{y^Tx-f(x)\}$，那么有 $x^*=\nabla f^*(y)\iff y=\nabla f(x^*)$。这可以用来求极值，比如 $\min f(x)\Longrightarrow 0=\nabla f(x)\iff x=\nabla f^*(0)$

### 1.3 例子

常用的共轭函数的例子有

**负对数函数** $f(x)=-\log x$
$$
\begin{aligned}
f^{*}(y) &=\sup _{x>0}(x y+\log x) \\
&=\left\{\begin{array}{ll}
-1-\log (-y) & y<0 \\
\infty & \text { otherwise }
\end{array}\right.
\end{aligned}
$$
**凸二次函数** $f(x)=(1 / 2) x^{T} Q x$ with $Q \in \mathbf{S}_{++}^{n}$
$$
\begin{aligned}
f^{*}(y) &=\sup _{x}\left(y^{T} x-(1 / 2) x^{T} Q x\right) \\
&=\frac{1}{2} y^{T} Q^{-1} y
\end{aligned}
$$
**指示函数** $I_S(x)=0$ on $\text{dom}I_S=S$
$$
I_S^*(y)=\sup\{y^Tx|x\in S\}
$$
**log-sum-exp 函数** $f(x)=\log\sum\exp x_i$
$$
f^{*}(y)=\left\{\begin{array}{ll}
\sum_{i=1}^{n} y_{i} \log y_{i} & \text { if } y \succeq 0 \text { and } \mathbf{1}^{T} y=1 \\
\infty & \text { otherwise. }
\end{array}\right.
$$
**范数** $f(x)=\Vert x\Vert$
$$
f^{*}(y)=\left\{\begin{array}{ll}
0 & \|y\|_{*} \leq 1 \\
\infty & \text { otherwise }
\end{array}\right.
$$
**范数平方** $f(x)=(1/2)\Vert x\Vert^2$
$$
f^*(y)=(1/2)\Vert y\Vert_*^2
$$
**负熵** $f(x)=\sum x_i\log x_i$
$$
f^*(y)=\sum e^{y_i-1}
$$

