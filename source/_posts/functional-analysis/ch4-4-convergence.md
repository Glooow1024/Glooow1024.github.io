---
title: 泛函分析笔记7：弱收敛与弱星收敛
toc: true
date: 2020-12-26 16:40:38
tags:
  - 强收敛
  - 弱收敛
  - 弱星收敛
  - 一致收敛
categories:
  - Functional Analysis
mathjax: true
---

一致有界性原理的一个应用就是序列和算子的收敛性分析。

## 1. 序列收敛性

$(X,\Vert\cdot\Vert)$，有 $x_n,x\in X$，称 $x_n$ **强收敛**到 $x$，若 $\Vert x_n-x\Vert \to 0$；称 $x_n$ **弱收敛**到 $x$ 若 $\forall f\in X'$ 都有 $f(x_n)\to f(x)$，记为 $x_n \stackrel{w}{\longrightarrow} x.$

关于弱收敛有以下几条性质：

- 若 $x_n \stackrel{w}{\longrightarrow} x, x_n \stackrel{w}{\longrightarrow} y$，则 $x=y$；
- 若 $x_n \stackrel{w}{\longrightarrow} x$，则存在 $c\ge0, \Vert x_n\Vert \le c.$

<!--more-->

证明：仅证第二条。这个性质说明 $x_n$ 有界，因此容易想到需要用一致有界性原理证明，但是该原理说明的是算子的一致有界，这里是元素 $x_n$ 有界，因此又可以想到上一篇讲到的典范映射 $J:X\to X''$ 从元素映射到算子。因此这里考虑 $X'$ 上的线性泛函 $g_n= J(x_n):X'\to \mathbb{R}$，有 $g_n(f)=f(x_n),\forall f\in X'.$ 于是有 $f(x_n)\to f(x)$，因而固定任一 $f$，都有 $\sup_n g_n(f) < \infty$，同时由于 $X'$ 总为 Banach 空间，利用一致有界性原理有 $\sup_n \Vert g_n\Vert =\sup_n \Vert x_n\Vert < \infty$。证毕。

> **定理**：$(X,\Vert\cdot\Vert)$，有 $x_n,x\in X$，则 $x_n \stackrel{w}{\longrightarrow} x$ **当且仅当**：
>
> 1. 存在 $c\ge0,\Vert x_n\Vert\le c$；
> 2. 并且存在 $M\subset X',\overline{\text{span}M}=X'$，对 $\forall f\in M, f(x_n)\to f(x).$（此时 $M$ 称为**完全集**）
>
> **NOTE**：该定理简化了弱收敛的判断条件，只需要在 $X'$ 的一个子集上判断函数值是否收敛。

证明：$"\Longrightarrow"$ 易证；

$"\Longleftarrow"$，首先考虑 $\forall f\in \text{span}M$，容易得到 $f(x_n)\to f(x)$。然后对 $\forall g\in X'$，那么存在 $f_m\in\text{span}M$ 使得 $\Vert f_m-g\Vert \le 1/m$，因此
$$
\begin{align}
|g(x_n)-g(x)|&\le |g(x_n)-f_m(x_n)|+|f_m(x_n)-f_m(x)|+|f_m(x)-g(x)| \\
&\le \frac{1}{m}(\Vert x_n\Vert+\Vert x\Vert)+|f_m(x_n)-f_m(x)| \to 0(m,n\to\infty)
\end{align}
$$
证毕。

***例子 1***：考虑 $X=\ell^p(1<p<\infty)$，有 $(\ell^p)'=\ell^q, 1/p+1/q=1.$ 考虑线性泛函 $f_y(x)=\sum_i y_ix_i,y\in \ell^q$，有 $\Vert f_y\Vert=\Vert y\Vert_q$。我们考虑 $X'$ 的子空间 $M=\{e_n,n\ge1\}$，其中 $e_n=(...,0,1,0,...)$ 表示只有第 $n$ 个分量为 1，其余为 0。那么 $\overline{\text{span}M}=X'$，因此要想验证 $x_n$ 是否弱收敛到 $x$ 就只需要验证：1）其有界性；2）对每个 $f_{e_k},k\ge1$ 是否有 $f_{e_k}(x_n)\to f_{e_k}(x)(n\to\infty).$

强收敛与弱收敛之间有如下关系：

- $x_n\to x \Longrightarrow x_n \stackrel{w}{\longrightarrow} x$(即强收敛可以导出弱收敛)；
- 若 $\text{dim}X<\infty$，则 $x_n \stackrel{w}{\longrightarrow} x\Longrightarrow x_n\to x$(**有限维**赋范空间中，强收敛与弱收敛等价)；

证明：仅证第二条。设 $\text{dim}X=n<\infty$，有限维赋范空间中我们可以找到一组基，$x_k=\lambda_{k,1}e_1+\cdots+\lambda_{k,n}e_n$，$x=\lambda_1 e_1+\cdots+\lambda_n e_n$。那么
$$
\lambda_{k,1}f(e_1)+\cdots+\lambda_{k,n}f(e_n) \to \lambda_{1}f(e_1)+\cdots+\lambda_{n}f(e_n), \quad\forall f\in X'
$$
由于 $f\in X'$ 任取，那么我们可以取 $f_i(y)=\mu_i$，其中 $y=\mu_1 e_1+\cdots+\mu_n e_n$，即 $f_i$ 取出来第 $i$ 个坐标系数。由此可以得到 $\lambda_{k,i}\to\lambda_i(k\to\infty)$，然后就容易得到 $x_n\to x.$ 证毕。

***例子 2***：有些无穷维空间中也可以得到 $x_n \stackrel{w}{\longrightarrow} x\iff x_n\to x$，例如 $\ell^1.$

***例子 3***：无穷维 Hilbert 空间（$\ell^2$，注意只有 $2-$范数才能定义出对应的内积），考虑 $\{e_1,e_2,\ldots\}$ 为 $H$ 的标准正交集，那么有 $e_n \stackrel{w}{\longrightarrow} 0$ 但是 $e_n \nrightarrow 0$。考虑 $\forall f\in H'$，存在唯一的 $z_0\in H, f(x)=\langle x,z_0\rangle$，由Bessel方程 $\sum_n|\langle e_n,z_0\rangle|^2\le \Vert z_0\Vert^2$，因此 $f(e_n)\to 0(n\to \infty),\forall f\in H'$，但另一方面 $\Vert e_n\Vert=1\nrightarrow0$。



## 2. 线性泛函收敛性

对于算子的收敛性，如线性泛函 $f_n\in X'$ 或者有界线性算子 $T\in B(X,Y)$，收敛性的定义跟上面序列的收敛性是相似的，但是又略有不同。下面就先给出线性泛函收敛性的分析。

同样考虑赋范空间 $X$，$f,f_n\in X'$，称 $f_n$ **弱星收敛**到 $f$，若任取 $x\in X$ 都有 $f_n(x)\to f(x)$，记为 $f_n \stackrel{w\star}{\longrightarrow} f.$

**NOTE**：实际上这里的弱星收敛跟序列的弱收敛是完全对称的，因此他们的性质也是类似的。

- 弱星收敛极限 $f$ 唯一；
- $\{f_n\}$ 的任意子列均弱星收敛到 $f$；
- 若 $X$ 为 Banach 空间，则 $\{f_n\}$ 在 $X'$ 中为**有界集**。

证明：仅证第三条，对于任意 $x\in X$，有 $f_n(x)\to f(x)$，因此 $f_n(x)$ 有界，由一致有界性原理，$\sup_n \Vert f_n\Vert<\infty.$ 证毕。

> **定理**：$X$ 为 **Banach 空间**，$f_n,f\in X'$，则 $f_n \stackrel{w\star}{\longrightarrow} f$ **当且仅当**：
>
> 1. 存在 $c\ge0,\Vert f_n\Vert\le c$；
> 2. 并且存在 $M\subset X,\overline{\text{span}M}=X$，对 $\forall x\in M, f_n(x)\to f(x).$
>
> **NOTE**：该性质与序列弱收敛的性质完全对称，证明省略。



> **NOTE**：对于 $X'$ 中的线性算子 $f$，也有范数的定义，因此我们也可以按照序列的收敛性来定义算子的收敛性。这个时候就用 $X'$ 代替上面的 $X$，用 $X''$ 代替上面的 $X'$。我们可以得到什么样的强收敛和弱收敛定义呢？（下面并不是标准的数学定义，只是我为了引出之后的内容做的解释）
>
> 对于 $f,f_n\in X'$，若满足 $\Vert f_n-f\Vert\to 0$，则称 $f_n$ **一致收敛**到 $f$；若对 $\forall g\in X''$，都有 $g(f_n)\to g(f)$，那么称 $f_n$ **强收敛**到 $f$；弱收敛的定义暂且不管。
>
> 注意从这个定义的字面意思来看，这里的一致收敛对应于上面序列的强收敛；这里的强收敛对应上面序列的弱收敛，它实际上也就对应于弱星收敛。这里就有两个值得思考的问题：1）**一致收敛和强收敛的区别是什么？**2）这里的强收敛为什么对应上面的弱收敛？
>
> 先看第2个问题：讲 Hahn-Banach 定理应用的时候我们讲到了典范映射，如果 $X$ 为自反的，那么任意一个 $g_0\in X''$ 都唯一的对应于 $X$ 中的元素 $x_0$，并且满足 $g_0(f)=f(x_0),\forall f\in X'$。假设 $X$ 是自反的，那么上面的强收敛定义就可以表述为 $\forall x\in X$，都有 $f_n(x)\to f(x)$，注意看！这是不是就是线性泛函弱星收敛的定义！也对应了序列的弱收敛。不过弱星收敛的定义里面并没有要求 $X$ 是自反的。
>
> 那么再看第1个问题：一致收敛中要求 $\Vert f_n-f\Vert\to0$，线性算子的范数是针对整个源空间考虑的；而强收敛中对每个 $x\in X$，关注 $f_n(x)\to f(x)$，也就是说关注的是每一个局部点。因此一致收敛要强于强收敛。



## 3. 一般有界线性算子收敛性

实际上一致收敛、强收敛、弱收敛的概念可以扩展到任意的有界线性算子定义。

设 $X,Y$ 为赋范空间，$T_n\in B(X,Y),T:X\to Y$ 为线性算子，有三种收敛性：

- $\{T_n\}$ **一致收敛**到 $T$，若 $\Vert T_n-T\Vert \to 0$；
- $\{T_n\}$ **强收敛**到 $T$，若 $\forall x\in X,T_n x\to Tx$；
- $\{T_n\}$ **弱收敛**到 $T$，若任取 $x\in X, f\in Y'$，$f(T_nx)\to f(Tx)$。

容易看出来一致收敛 $\Longrightarrow$ 强收敛 $\Longrightarrow$ 弱收敛，但是反向则不成立，可以举出对应的反例。

***例子 4***(强收敛 $\nRightarrow$ 一致收敛)：$X=Y=\ell^2$，$T_n:\ell^2\to\ell^2$ 有
$$
T_n: (x_1,x_2,\cdots) \mapsto (0,\cdots,0,x_{n+1},x_{n+2},\cdots)
$$
容易验证 $T_n$ 为有界线性算子，$\Vert T_n\Vert=1$。可以验证 $T_n$ 强收敛到 $0$ 算子，即 $T_0x\equiv 0$。但是 $\Vert T_n-T_0\Vert=1\nrightarrow 0$，即不满足一致收敛。

***例子 5***(弱收敛 $\nRightarrow$ 强收敛)：$X=Y=\ell^2$，$T_n:\ell^2\to\ell^2$ 有
$$
T_n:(x_1,x_2,\cdots)\mapsto(0_1,\cdots,0_n,x_1,x_2,\cdots)
$$
可以验证 $T_n$ 为有界线性算子，并且 $\Vert T_n\Vert=1$。是否有 $T_n$ 弱收敛到某个 $T$ 呢？考虑任意 $f\in(\ell^2)'$，都存在唯一的 $z\in\ell^2$，$f(x)=\langle x,z\rangle$，所以 $f(T_nx)=x_1\overline{z_{n+1}}+x_2\overline{z_{n+2}}+\cdots$，因此 
$$
|f(T_nx)|\le\sum_{k=1}^\infty |x_k|\cdot|z_{n+k}| \le \Vert x\Vert\left(\sum_{k=n+1}^\infty |z_k|^2\right)^{1/2} \to 0
$$
所以有 $f(T_nx) \to 0$ 对任意 $f\in (\ell^2)'$ 成立，因此 $f(T_nx)\to f(T_0x)\equiv f(0)=0$。所以 $T_n$ 弱收敛到 $T_0=0$ 算子，但是总有 $\Vert T_nx\Vert=\Vert x\Vert\nrightarrow 0$，因此 $T_nx\nrightarrow T_0x$，即不满足强收敛。

**命题**：对于一般有界线性算子，若 $T_n$ **一致收敛**到 $T$，则 $T$ 也是有界的，这是因为 $\Vert T\Vert\le \Vert T-T_n\Vert+\Vert T_n\Vert \le \infty$；若只能得到 $T_n$ **强收敛**到 $T$，那么 $T$ **不一定是有界**的。

***例子 6***(强收敛极限未必有界)：$X=Y=\{(x_n),\exists N,\forall n\ge N, x_n=0 \}$，考虑 $T_n:X\to Y$ 有
$$
\begin{aligned}
T_n:& (x_1,x_2,\cdots)\mapsto (x_1,2x_2,\cdots,nx_n,x_{n+1},x_{n+2},\cdots) \\
T:& (x_1,x_2,\cdots)\mapsto (x_1,2x_2,\cdots)
\end{aligned}
$$
那么 $\Vert T_n\Vert=n$，取可以验证对于 $\forall x\in X$，$T_nx\to Tx$，即 $T_n$ 强收敛到 $T$，但是 $T$ 不是有界算子。

那么什么情况下可以保证强/弱收敛极限也是有界算子呢？

> **定理**：设 $X$ 为 **Banach 空间**，$Y$ 为赋范空间，$T_n\in B(X,Y),T:X\to Y$ 为线性算子。设 $T_n$ **弱收敛**到 $T$，则 $\sup_{n\ge1}\Vert T_n\Vert < \infty, T\in B(X,Y)$ 并且 $\Vert T\Vert \le \sup_{n\ge1}\Vert T_n\Vert <\infty.$

**证明**：由于 $T_n$ 弱收敛到 $T$，即 $\forall x\in X,f\in Y'$ 都有 $f(T_nx)\to f(Tx)$，因此有 $T_nx \stackrel{w}{\longrightarrow} Tx$。那么根据序列弱收敛的性质，存在 $c_x$ 满足 $\sup_n \Vert T_nx\Vert \le c_x$，再由一致有界性原理，有 $\sup_n \Vert T_n\Vert < \infty$。

然后考虑 $T$，$\forall x\in X$，由 Hahn-Banach 定理的推论，都存在 $f\in Y',\Vert f\Vert=1$ 满足 
$$
\Vert Tx\Vert=|f(Tx)| = \lim_{n\to\infty} |f(T_nx)| \le \lim_{n\to\infty}\Vert T_nx\Vert
$$
因此 $\Vert T\Vert\le \sup_n\Vert T_n\Vert.$ 证毕。

> **定理**：设 $X$ 为 **Banach 空间**，$Y$ 为赋范空间，$T_n,T\in B(X,Y)$，则 $T_n$ **强收敛**到 $T$ **当且仅当**：
>
> 1. $\sup_n \Vert T_n\Vert < \infty$；
> 2. 存在 $M\subset X,\overline{\text{span}M}=X$，对 $\forall x\in M, T_n(x)\to T(x).$
>
> **NOTE**：这跟线性泛函弱星收敛的等价条件是完全一样的，证明省略。



## 4. 应用举例

***例子 7***(求积分的数值方法)：考虑实值函数 $x\in C[a,b]$，并赋予无穷范数，那么 $(C[a,b],\Vert\cdot\Vert)$ 为 Banach 空间，求 $\int_a^b x(t)dt.$

既然是在本节举的这个例子，那就要用到算子收敛性。先定义有界线性算子 $f(x)=\int_a^b x(t)dt$，$\Vert f\Vert=b-a$。我们现在的目标就是找一列有界线性泛函 $f_n$ 弱收敛到 $f$。回忆我们在学微积分的时候，往往是用分段的矩形面积求和来逼近积分。在 $[a,b]$ 上取 $n+1$ 个结点 $a=t_{n,0}<t_{n,1}<\cdots<t_{n,n}=b$，再取 $n+1$ 个实数 $a_{n,0},\cdots,a_{n,n}$，令
$$
f_n(x) = \sum_{k=0}^n a_{n,k} x(t_{n,k})
$$
$f_n$ 是 $C[a,b]$ 上的线性泛函，并且 $\Vert f_n\Vert \le \sum_{k=0}^n|a_{n,k}|$，另外我们总能够造出一个 $x\in C[a,b]$ 满足 $x(t_{n,k})=\text{sgn}(a_{n,k})$ 并且 $\Vert x\Vert_\infty=1$，此时就有 $f(x)=\sum_{k=0}^n|a_{n,k}|$，于是可以得到 $\Vert f_n\Vert = \sum_{k=0}^n|a_{n,k}|$。现在的问题就是我们能否找到合适的系数 $a_{n,k}$ 使得 $f_n\stackrel{w}{\longrightarrow} f$ ？

这里我们提出一个额外的要求，就是对于次数小于 $n$ 的多项式 $p$，需要 $f_n(p)$ 能获得精确积分结果，即 $f_n(p)=\int_a^b p(t)dt$。由于 $\{1,t,\ldots,t^n\}$ 构成次数小于 $n$ 的多项式空间的 Hamel 基，所以只需要验证对每个基有 $f_n(e_k)= f(e_k)$ 即可。这就要求
$$
\begin{cases}
\begin{matrix}
a_{n,0} & + & a_{n,1} & + & \cdots & + & a_{n,n} & = & b-a \\
a_{n,0}t_{n,0} & + & a_{n,1}t_{n,1} & + & \cdots & + & a_{n,n}t_{n,n} & = & \frac{b^2-a^2}{2} \\
& & & & \ldots & \\
a_{n,0}t_{n,0}^n & + & a_{n,1}t_{n,1}^n & + & \cdots & + & a_{n,n}t_{n,n}^n & = & \frac{b^{n+1}-a^{n+1}}{n+1}
\end{matrix}
\end{cases}
$$
上式左侧可以用 Vandermonde 矩阵表示，因此存在唯一解 $a_{n,k},k=1,...,n$。

接下来对于任意的 $x\in C[a,b]$，能否找到 $a_{n,k}$ 满足的条件使得 $f_n(x)\to f(x)$ 呢？根据 Stone-Weierstrass 定理，多项式的集合在 $C[a,b]$ 中是**稠密的**，因此对于任意次数 $N$ 的多项式 $p$ 总有 $f_n(p)\to f(p)$。那么再应用前面的定理（即只需要判断完全集 $M\subset X$ 中的元素是否满足条件即可），可以有如下结论

**定理(G.Polya)**：设数值积分 $f_n$ 满足前面对于有限次多项式的要求（即 $a_{n,k}$ 为 Vandermonde 矩阵方程的解）则任取 $x\in C[a,b],f_n(x)\to f(x)$ 当且仅当存在常数 $C\ge0$，使得任取 $n\ge1$，有 $\sum_{k=1}^na_{n,k}\le C.$




