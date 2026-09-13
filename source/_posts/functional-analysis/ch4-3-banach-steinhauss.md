---
title: 泛函分析笔记6：一致有界性原理
toc: true
date: 2020-12-26 16:36:30
tags:
  - Baire范畴定理
  - 无处稠密
  - 第一范畴、第二范畴
  - 一致有界性原理
  - 共鸣定理
categories:
  - Functional Analysis
mathjax: true
---

Hahn-Banach定理主要是用于泛函的延拓，在较小的子空间上满足某个性质之后我们就可以将对应的泛函延拓至整个空间。而这一节要讲的一致有界性原理恰如其名，主要讨论一族有界线性算子一致有界的条件。他也是后续讨论序列弱收敛性以及泛函弱星收敛性的基础。

## 1. Baire范畴定理

一致有界性原理的证明需要用到Baire范畴定理（也叫Baire纲定理）。

$(X,d)$，若 $\bar{M}\subset X$ 没有内点，则称 $M$ 是**无处稠密**的。若 $N=\cup^\infty_n N_n$，$N_n$ 均为无处稠密的，则称 $N$ 为**第一范畴**。不为第一范畴的子集称为**第二范畴**。

<!--more-->

*例子 1*：$X=\mathbb{R},d(s,t)=|s-t|$，任意有限集均为无处稠密的，因此可数集均为第一范畴。

> **定理(范畴定理)**：$(X,d)$ 为**非空完备**的，则 $X$ 必为**第二范畴**的。

**证明**：第二范畴意味着 $X$ 不能表示为可数个没有内点的集合的并集。假设 $X$ 属于第一范畴，即 $X=\cup_n M_n$，并且不妨设 $M_n$ 均为闭集（否则可以取 $X=\cup_n  \bar{M}_n$），并且 $M_n$ 都没有内点。

首先考虑 $Y_1=M_1^c$ 为开集，因此存在某个 $x_1\in Y_1,r_1\in(0,1/2)$ 使得 $B(x_1,r_1)\subset Y_1$。由于 $M_2$ 也没有内点并且为闭集，因此 $Y_2=B(x_1,r_1) \cap M_2^c \ne \varnothing$ 也为开集，因此可以找到某个 $x_2\in Y_2,r_2\in(0,r_1/2)$ 使得 $B(x_2,r_2)\subset Y_2$。依此类推，可以找到 $B(x_1,r_1) \supset \cdots \supset B(x_n,r_n)\supset \cdots$，并且有 $r_n\in (0,1/2^n)$。容易验证 $\{x_n\}$ 为柯西列，因此存在收敛值 $x_n\to x,x\in X$。对于 $k\ge1$ 考虑 $x_{n+k},x\in \bar{B}(x_n,r_n/2)\subset B(x_n,r_n)$，于是有 $x\in B(x_n,r_n),\forall n\ge1$，因此就有 $x\notin M_n,\forall n\ge1$，因此 $x\notin X$，导出矛盾。证毕。



## 2. 一致有界性原理

> **一致有界性原理**：假设 $X$ 为 **Banach 空间**，$Y$ 为赋范空间，$T_i\in B(X,Y),\forall i\in \mathcal{I}$，并且对任取 $x\in X$ 有
> $$
> \sup_{i\in\mathcal{I}} \Vert T_ix\Vert < \infty
> $$
> 则 $\sup_{i\in\mathcal{I}}\Vert T_i\Vert<\infty.$
>
> **NOTE**：条件当中针对的是固定任意一个 $x\in X$，$T_i x$ 有界，也即是说所有的 $T_ix,i\in\mathcal{I}$ 存在一个上界 $c_x$，该上界与 $x$ 有关。对于线性泛函，我们只需要考虑 $\Vert x\Vert=1$ 的情况，但即便如此，一般而言由该条件并不能推导出对于所有的 $\Vert x\Vert, c_x$ 存在一个共同的上界，因为随着 $x$ 的变化 $c_x$ 有可能趋于无穷。而一致有界性原理则说明当 $X$ 为 Banach 空间的时候，一定存在这样一个上界，从而说明 $\Vert T_i\Vert$ 有上确界。

**证明**：根据上面的分析，我们在寻找 $\sup_i\Vert T_i\Vert$ 的时候不能局限在 $\Vert x\Vert=1$ 的情况，下面的证明方法很巧妙。

首先考虑 $X$ 完备，根据 Baire 范畴定理，不能表示为可数个没有内点的集合的并集。那么假如我们将其表示为可数个集合的并集，则一定存在某个集合有内点。因此考虑 $M_n=\{x\in X, \sup_i\Vert T_ix\Vert \le n\}$，因此就有 $X=\cup_n^\infty M_n$，一定存在某个 $N\ge1$ 使得 $M_N$ 有内点，此时找到 $x_0\in M_N,r>0$ 使得 $\bar{B}(x_0,r)\subset M_N$，并且 $\forall y\in X,\Vert y\Vert=1$，都有
$$
\Vert T_i(x_0)\Vert \le N,\ \Vert T_i(x_0+ry)\Vert \le N,\quad \forall i\in\mathcal{I},\Vert y\Vert=1 \\
\Longrightarrow \Vert T_iy\Vert \le 2N/r, \quad \forall i\in\mathcal{I},\Vert y\Vert=1 \\
\Longrightarrow \sup_{i\in\mathcal{I}}\Vert T_i\Vert \le 2N/r
$$
证毕。

> **共鸣定理**：假设 $X$ 为 **Banach 空间**，$Y$ 为赋范空间，$T_i\in B(X,Y),\forall i\in \mathcal{I}$，设 $\sup_i\Vert T_i\Vert = \infty$，则 $\exists x\in X$ 使得 $\sup_{i} \Vert T_i x\Vert= \infty$，其中 $x$ 即为 $T_i$ 的共鸣点。
>
> **NOTE**：实际上共鸣定理就是一致有界性原理的逆反命题。



## 3. 应用举例

一致有界性原理的 Banach 空间假设是必不可少的，下面的例子将进行解释。

***例子 2***：$X$ 为所有的多项式，即 $p\in X$ 可以表示为 $p(t)=a_0+a_1 t+\cdots a_N t^N$，定义 $\Vert p\Vert =\max_{0\le i\le N}|a_i|$。取一列线性泛函 $f_n(p)=a_0+a_1+\cdots a_n$，那么可以验证 $f_n\in X'$ 并且有 $\Vert f_n\Vert=n+1$。

此时显然我们有对任意固定的 $p=a_0+a_1 t+\cdots a_N t^N\in X$，$\Vert f_n(p)\Vert \le |a_0|+\cdots+|a_N|$，但是同时我们也有 $\sup_n \Vert f_n\Vert=\infty$，这似乎与一致有界性原理矛盾？其实原因是 $X$ 并不是 Banach 空间。

考虑 $p_n(t)=1+\frac{1}{2}t+\cdots+\frac{1}{n+1}t^n$，那么可以验证 $\{p_n\}$ 为柯西列，但是其收敛值却并不在 $X$ 内部（$X$ 中只包含有限项的多项式），因此 $X$ 不完备。

一致有界性原理还可用于讨论 Fourier 级数的收敛问题。

***例子 3***：考虑 $t\in[0,2\pi]$，对于 $[0,2\pi]$ 上的周期函数，很多时候我们用 Fourier 级数来表示他们，即
$$
\begin{aligned}
a_0(x) &= \frac{1}{2\pi}\int_0^{2\pi} x(t)dt \\
a_m(x) &= \frac{1}{\pi}\int_a^{2\pi} x(t)\cos(mt)dt, m\ge1 \\
b_m(x) &= \frac{1}{\pi}\int_a^{2\pi} x(t)\sin(mt)dt, m\ge1
\end{aligned}
$$
 $x_n(t)=a_0+\sum_{m=1}^n\left(a_m \cos mt+b_m\sin mt\right)$ 即为 Fourier 级数。但是一个问题就是 Fourier 级数是否点点收敛到 $x$？答案是否定的，可以用一致有界性原理证明。

证明：考虑 $f_n(x)=x_n(0)=a_0(x)+\sum_{m=1}^n a_m(x)$，显然 $f_n(x)$ 为 $C_{per}[0,2\pi]$（连续周期函数）上的线性泛函。并且有
$$
\begin{aligned}
f_n(x)&=\frac{1}{2\pi}\int_0^{2\pi} \left(1+2\sum_{m=1}^n \cos(mt) \right)x(t)dt \\
&= \frac{1}{2\pi}\int_0^{2\pi} \frac{\sin(n+1/2)t}{\sin(t/2)}x(t)dt= \frac{1}{2\pi}\int_0^{2\pi} Q_n(t)x(t)dt \\
\end{aligned}
$$
因此 $\Vert f_n\Vert \le \int_0^{2\pi} |Q_n(t)|dt$。由于 $Q_n(t)$ 在 $[0,2\pi]$ 上只有有限个零点，因此可以构造合适的 $x$ 证明 $\Vert f_n=\Vert \ge \frac{1}{2\pi}\int_0^{2\pi} |Q_n(t)|dt$。而
$$
\begin{aligned}
\int_0^{2\pi} |Q_n(t)|dt &\ge 2\int_0^{(2n+1)\pi} \frac{|\sin t|}{t}dt \ge2\sum_{k=0}^{2n}\int_{k\pi}^{(k+1)\pi} \frac{|\sin t|}{t}dt \\
&\ge \sqrt{2} \sum_{k=0}^{2n}\int_{k\pi+\pi/4}^{k\pi+3\pi/4} \frac{1}{t}dt \\
&\ge \sum_{k=0}^{2n} \frac{4\sqrt{2}}{8k+3}
\end{aligned}
$$
因此有 $\sup_{n\ge1}\Vert f_n\Vert=\infty$，由一致有界性原理，存在 $x_0\in C_{per}[0,2\pi]$ 使得 $\sup_{n\ge1} |f_n(x_0)|=\infty$，因此 $x_0$ 的 Fourier 级数在 $t=0$ 处不收敛。证毕。
