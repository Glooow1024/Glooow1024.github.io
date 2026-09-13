---
title: 凸优化笔记 11：对偶原理 & 拉格朗日函数
date: 2020-03-18 22:46:26
tags:
  - 对偶原理
  - 拉格朗日函数
  - SCQ
categories:
  - Convex Optimization
mathjax: true
---

前面讲了凸优化问题的定义，以及一些常见的凸优化问题类型，这一章就要引入著名的拉格朗日函数和对偶问题了。通过对偶问题，我们可以将一些非凸问题转化为凸优化问题，还可以求出原问题的非平凡下界，这对复杂优化问题是很有用的。

<!--more-->

## 1. 拉格朗日函数

考虑凸优化问题
$$
\begin{aligned}
\text { minimize } \quad& f_{0}(x)\\
\text { subject to } \quad& f_{i}(x) \leq 0, \quad i=1, \ldots, m\\
&h_{i}(x)=0, \quad i=1, \ldots, p
\end{aligned}
$$
假设 $x\in R^n$，定义域为 $\mathcal{D}$，最优解为 $p^\star$。

我们定义**拉格朗日函数(Lagrangian)**为 $L:R^n\times R^m\times R^p\to R$，$\text{dom}L=\mathcal{D}\times R^m\times R^p$
$$
L(x,\lambda,\nu)=f_0(x)+\lambda^Tf(x)+\nu^Th(x)
$$
再取下确界得到**拉格朗日对偶函数(Lagrange dual function)** $g:R^m\times R^p\to R$
$$
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}\left(f_0(x)+\lambda^Tf(x)+\nu^Th(x)\right)
$$
这个拉格朗日对偶函数可不得了啦！他有两个很重要的性质：

> 1. $g(\lambda,\nu)$ 是**凹函数**（不论原问题是否为凸问题）
> 2. 如果 $\lambda\succeq 0$，那么 $g(\lambda,\nu)\le p^\star$（对任意 $\lambda\succeq0,\nu$ 都成立）

**Remarks**：上面两个性质为什么重要呢？首先由于 $g(\lambda,\nu)\le p^\star$，这可以给出原问题最优解的一个**不平凡下界**，这意味着如果原问题很难求解的时候，我们可以转变思路，求解一个新的优化问题：
$$
\begin{aligned}
\text { maximize } \quad& g(\lambda,\nu)\\
\text { subject to } \quad& \lambda\succeq0
\end{aligned}
$$
另一方面，由于不论原函数是否为凸优化问题，新的问题都是凸的，因此可以方便求解。下面举几个例子。

***例子 1***：原问题为
$$
\begin{aligned}
\text { maximize } \quad& x^Tx\\
\text { subject to } \quad& Ax=b
\end{aligned}
$$
那么可以很容易得到拉格朗日函数为 $L(x,\nu)=x^Tx+\nu^T(Ax-b)$，对偶函数为 $g(\nu)=-(1/4)\nu^TAA^T\nu-b^T\nu$，也即

$p^\star\ge g(\nu)$。

***例子 2***：标准形式的线性规划(LP)
$$
\begin{aligned}
\text { maximize } \quad& c^Tx\\
\text { subject to } \quad& Ax=b,\quad x\succeq0
\end{aligned}
$$
按照定义容易得到对偶问题为
$$
\begin{aligned}
\text { maximize } \quad& -b^T\nu\\
\text { subject to } \quad& A^T\nu+c\succeq0
\end{aligned}
$$
***例子 3***：原问题为最小化范数
$$
\begin{aligned}
\text { maximize } \quad& \Vert x\Vert\\
\text { subject to } \quad& Ax=b
\end{aligned}
$$
对偶函数为
$$
g(\nu)=\inf_{x} (\Vert x\Vert+\nu^T(b-Ax))
=\begin{cases}b^T\nu & \Vert A^T\nu\Vert_* \le1 \\ -\infty & o.w.\end{cases}
$$
这个推导过程中用到了**共轭函数**的知识。实际上上面三个例子都是线性等式约束，这种情况下，我们应用定义推导过程中可以很容易联想到共轭函数。（实际上加上线性不等式约束也可以）

***例子 4***：(原问题非凸)考虑 Two-way partitioning (不知道怎么翻译了...)
$$
\begin{aligned}
\text { maximize } \quad& x^TWx\\
\text { subject to } \quad& x_i^2=1,\quad i=1,...,n
\end{aligned}
$$
对偶函数为
$$
\begin{aligned}
g(\nu)&=\inf_{x}\left( x^{T}(W+\operatorname{diag}(\nu)) x \right)-\mathbf{1}^{T} \nu \\
&=\left\{\begin{array}{ll}
-\mathbf{1}^{T} \nu & W+\operatorname{diag}(\nu) \succeq 0 \\
-\infty & \text { otherwise }
\end{array}\right.
\end{aligned}
$$
于是可以给出原问题最优解的下界为 $p^\star\ge-\mathbf{1}^{T} \nu$ if $W+\operatorname{diag}(\nu) \succeq 0$。这个下界是不平凡的，比如可以取 $\nu=-\lambda_{\min}(W)\mathbf{1}$，可以给出 $p^\star\ge n\lambda_{\min}(W)$。

## 2. 对偶问题

上面已经多次提到**对偶问题(Lagrange dual problem)**了
$$
\begin{aligned}
\text { maximize } \quad& g(\lambda,\nu)\\
\text { subject to } \quad& \lambda\succeq0
\end{aligned}
$$
假如对偶问题的最优解为 $d^\star=\max g(\lambda,\nu)$，那么我们有 $p^\star \ge d^\star$。

现在我们当然想知道什么情况下可以取等号，也即 $p^\star = d^\star$，此时我们只需要求解对偶问题就可以获得原问题的最优解了。在此之前，我们先引入两个概念：强对偶和弱对偶。

**弱对偶(weak duality)**：满足 $p^\star \ge d^\star$，原问题不论是否为凸，弱对偶总是成立；

**强对偶(strong duality)**：满足 $p^\star = d^\star$，强对偶并不总是成立，如果原问题为凸优化问题，一般情况下都成立。在凸优化问题中，保证强对偶成立的条件为被称为 **constraint qualiﬁcations**。

有很多种不同的 constraint qualiﬁcations，常用到的一种为 **Slater’s constraint qualiﬁcation(SCQ)**，其表述为

> **SCQ**：对于凸优化问题
> $$
> \begin{aligned}
> \text { minimize } \quad& f_{0}(x)\\
> \text { subject to } \quad& f_{i}(x) \leq 0, \quad i=1, \ldots, m\\
> &Ax=b
> \end{aligned}
> $$
> 如果存在可行解 $x\in\text{int}\mathcal{D}$，使得
> $$
> Ax=b,\quad f_i(x)<0,\quad,i=1,...,m
> $$
> 那么就能保证强对偶性。
>
> **Remarks**：
>
> - 由于存在线性等式约束，因此实际定义域可能不存在内点，可以将这一条件放松为相对内点 $x\in\text{relint}\mathcal{D}$；
> - 如果不等式约束中存在线性不等式，那么他也不必严格小于0。也即如果 $f_i(x)=C^Tx+d$，则只需要满足 $f_i(x)\le0$ 即可。

下面再举几个例子，看一看他们的 SCQ 条件是什么。

***例子 1***：还是考虑线性规划(LP) 或者二次规划(QP)
$$
\begin{aligned}
\text { minimize } \quad& c^Tx \quad(\text{ or }x^TPx)\\
\text { subject to } \quad& Ax\preceq b
\end{aligned}
$$
那么根据 SCQ 可以得到，如果想得到强对偶性，应该有 $\exist x, \text{ s.t. } Ax\preceq b$。

***例子 2***：(原问题非凸) Trust Region Methods
$$
\begin{aligned}
\text { minimize } \quad& x^TAx+2b^Tx\\
\text { subject to } \quad& x^Tx\le1
\end{aligned}
$$
其中 $A\nsucceq 0$，因此原问题不是凸的。他的对偶函数就是
$$
g(\lambda)=\inf_x\left(x^T\left(A+\lambda I\right)x+2b^Tx-\lambda\right)
=\begin{cases}-b^T(A+\lambda I)^\dagger b-\lambda & A+\lambda I\succeq0,b\in \mathcal{R}(A+\lambda I) \\ -\infty & o.w. \end{cases}
$$
注意如果不满足 $A+\lambda I\succeq0$ 或 $b\in \mathcal{R}(A+\lambda I)$，则 $g(\lambda)\to-\infty$。那么就可以得到对偶问题为
$$
\begin{aligned}
\text {maximize} \quad& -b^{T}(A+\lambda I)^{\dagger} b -\lambda\\
\text {subject to} \quad& A+\lambda I \succeq 0\\
&b \in \mathcal{R}(A+\lambda I)
\end{aligned}
$$
也可以等价转换为 SDP
$$
\begin{aligned}
\text {maximize} \quad& -t-\lambda\\
\text {subject to}\quad& \left[\begin{array}{cc}A+\lambda I & b \\ b^{T} & t\end{array}\right] \succeq 0
\end{aligned}
$$

> **Remarks**：这里用到了舒尔补(Schur complement)的知识。考虑矩阵
> $$
> X = \left[\begin{array}{cc}A & B \\ B^{T} & C\end{array}\right]
> $$
> 其中 $\det A\ne0,S=C-B^TA^{-1}B$。那么有以下及条性质：
>
> - $X\succ0 \iff A\succ0,S\succ0$
> - 若 $A\succ0$，则 $X\succeq0 \iff S\succeq 0$
> - $X\succeq0 \iff A\succeq0,(I-AA^\dagger)B=0,S=C-B^TA^{\dagger}B\succeq0$
>
> 关于第 3 条中的第二个要求 $(I-AA^\dagger)B=0$，对 $A$ 进行奇异值分解，有 $A=U\Sigma V$，那么我们对任意 $v$，有 $(I-AA^\dagger)Bv=(I-UU^T)Bv=0$，而 $UU^T$ 实际上就是向 $\mathcal{R}(A)$ 的投影矩阵，因此就要求 $Bv\in\mathcal{R}(A)$。

## 3. SCQ 几何解释

前面给出的是 SCQ 的代数描述，那么如何证明呢？另外如何从几何角度直观理解呢？

首先我们可以考虑最简单的优化问题
$$
\begin{aligned}
\text { minimize } \quad& f_0(x)\\
\text { subject to } \quad& f_1(x)
\end{aligned}
$$
定义集合 $\mathcal{G}=\{(f_1(x),f_0(x))|x\in\mathcal{D}\}$，那么对偶函数为
$$
g(\lambda)=\inf_{(u,t)\in\mathcal{G}}(t+\lambda u)
$$
如果我们画出下面这张图，阴影部分就是可行区域 $\mathcal{G}$，而 $(\lambda,1)^T$ 则正好定义了一个支撑超平面，$g(\lambda)$ 就等于 $t$ 轴的交点。通过取不同的 $\lambda$ 我们就可以得到不同的支撑超平面，也可以得到不同的 $g(\lambda)$，最终会有某一个 $\lambda^\star$ 对应的是 $d^\star=g(\lambda^\star)$。还需要注意这里的支撑超平面永远不可能是竖直的。

| ![dual geometry](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-geo.PNG) | ![dual geometry](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-geo2.PNG) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| $(\lambda,1)^T$ 正好定义了一个支撑超平面                     | 每个 $\lambda$ 对应一个支撑超平面                            |

那么 $p^\star$ 体现在哪个点呢？由于对于原优化问题，我们有 $f_1(x)\le0$，因此体现在这个图里面就是 $u\le0$，也就是上面左图当中的红色区域，而 $p^\star=\min f_0(x)=\min t$。

理解了这张图，我们现在开始证明两件事：

1. 证明弱对偶性，也即 $p^\star \ge d^\star$；
2. 证明强对偶性条件 SCQ。

注：在此之前，我们不妨加入等式约束，也即 $g(\lambda,\mu)=\inf_{(u,v,t)\in\mathcal{G}}(t+\lambda^T u+\mu^T v)$。

**弱对偶性的证明**：我们有 $\lambda\ge0$
$$
\begin{aligned}
p^\star &= \inf\{t|(u,v,t)\in\mathcal{G},u\le0,v=0\} \\
&\ge \inf\{t+\lambda^Tu+\mu^Tv|(u,v,t)\in\mathcal{G},u\le0,v=0\} \\
&\ge \inf\{t+\lambda^Tu+\mu^Tv|(u,v,t)\in\mathcal{G}\} \\
&= g(\lambda,\mu)
\end{aligned}
$$
**强对偶性条件 SCQ 的证明**：由 $g(\lambda,\mu)=\inf_{(u,v,t)\in\mathcal{G}}(t+\lambda^T u+\mu^Tv)$ 可以得到
$$
(\lambda,\mu,1)^T(u,v,t)\ge g(\lambda,\mu),\quad \forall (u,v,t)\in\mathcal{G}
$$
这实际上定义了 $\mathcal{G}$ 的一个超平面。特别的有 $(0,0,p^\star)\in\text{bd}\mathcal{G}$，因此也有
$$
(\lambda,\mu,1)^T(0,0,p^\star)\ge g(\lambda,\mu)
$$
这个不等式可以自然地导出弱对偶性，当“=”成立时则可以导出强对偶性。那么什么时候取等号呢？点 $(0,0,p^\star)$ 为**支撑点**的时候！也就是说

> 如果在边界点 $(0,0,p^\star)$ 处存在一个**非竖直的支撑超平面**，那么我们就可以找到 $\lambda,\mu$ 使得上面的等号成立，也就是得到了强对偶性。

注意前面的分析中我们并没有提到 SCQ，那么 SCQ 是如何保证强对偶性的呢？注意 SCQ 要求存在 $x\in\mathcal{D}$ 使得 $f(x)<0$，这也就意味着 $\mathcal{G}$ 在 $u< 0$ 半平面上有点，因此如果支撑超平面存在的话，就一定不是垂直的。

但这又引出另一个问题，那就是支撑超平面一定存在吗？答案是一定存在，这是由原问题的凸性质决定的。为了证明这一点，我们可以引入一个类似于 epigraph 的概念：
$$
\begin{aligned}
\mathcal{A} &= \mathcal{G} + (R^m_+\times \{0\}\times R_+) \\
&= \left\{(u,v,t) |\ \exist x\in\mathcal{D},s.t. f(x)\le u,h(x)=v,f_0(x)\le t\right\}
\end{aligned}
$$
由于原优化问题为凸的，可以应用定义证明集合 $\mathcal{A}$ 也是凸的，同时 $(0,0,p^\star)\in\text{bd}\mathcal{A}$，那么集合 $\mathcal{A}$ 在 $(0,0,p^\star)$ 点就一定存在一个支撑超平面。又由 SCQ 可知这个支撑超平面一定不是竖直的，因此就可以得到强对偶性了。

注：$(\lambda,\mu,1)^T(u,v,t)\ge g(\lambda,\mu),\quad \forall (u,v,t)\in\mathcal{A}$ 也成立。



## 4. 广义不等式约束与SDP

前面讨论拉格朗日函数的时候都只考虑了标量函数，如果约束函数为**广义不等式**，也即
$$
\begin{aligned}
\text { minimize } \quad& f_{0}(x)\\
\text { subject to } \quad& f_{i}(x) \preceq_{K_i} 0, \quad i=1, \ldots, m\\
&h_{i}(x)=0, \quad i=1, \ldots, p
\end{aligned}
$$
那么他的拉格朗日函数就是
$$
L\left(x, \lambda_{1}, \cdots, \lambda_{m}, \nu\right)=f_{0}(x)+\sum_{i=1}^{m} \lambda_{i}^{T} f_{i}(x)+\sum_{i=1}^{p} \nu_{i} h_{i}(x)
$$
对偶函数就是
$$
g\left(\lambda_{1}, \ldots, \lambda_{m}, \nu\right)=\inf _{x \in \mathcal{D}} L\left(x, \lambda_{1}, \cdots, \lambda_{m}, \nu\right)
$$
其同样满足 $p^\star\ge g\left(\lambda_{1}, \ldots, \lambda_{m}, \nu\right)$。对偶问题为
$$
\begin{aligned}
\text {maximize} \quad& g\left(\lambda_{1}, \ldots, \lambda_{m}, \nu\right) \\
\text {subject to}\quad& \lambda_i\succeq_{K_i^*}0,i=1,...,m
\end{aligned}
$$
强对偶性以及 Slater's Condition 是类似的。

对于 **SDP 问题**
$$
\begin{aligned}
\text {maximize} \quad& c^Tx \\
\text {subject to}\quad& x_1F_1+\cdots +x_nF_n\preceq G
\end{aligned}
$$
拉格朗日函数就是
$$
L(x, Z)=c^{T} x+\operatorname{tr}\left(Z\left(x_{1} F_{1}+\cdots+x_{n} F_{n}-G\right)\right)
$$
对偶函数为
$$
g(Z)=\inf _{x} L(x, Z)=\left\{\begin{array}{ll}
-\operatorname{tr}(G Z) & \operatorname{tr}\left(F_{i} Z\right)+c_{i}=0, \quad i=1, \ldots, n \\
-\infty & \text { otherwise }
\end{array}\right.
$$
对偶问题就是
$$
\begin{aligned}
\text {maximize} \quad& -\operatorname{tr}(G Z)\\
\text {subject to} \quad& Z \succeq 0, \quad \operatorname{tr}\left(F_{i} Z\right)+c_{i}=0, \quad i=1, \ldots, n
\end{aligned}
$$
强对偶性以及 Slater's Condition 是类似的。



## 5. 对偶问题的强对偶性与可行性

注意我们说**强对偶性**需要**严格满足**不等式约束(也即最优解需要满足 $h(x^\star)<0$ 而不能是 $h(x^\star)\le0$)，但如果存在线性不等式约束，则可以取到等号(也即 $Ax^\star+b\le0$)。这就会出现下面的现象：

1. 对于 **LP** 问题，由于约束是线性的，因此强对偶性只要求有可行解，而不要求 **strictly feasible**；
2. 对于其他问题，若存在非线性约束，比如 **SOCP/SDP** 问题，如果想要满足强对偶性，就需要满足 **strictly feasible**，这就会出现两种情况：1）问题本身的可行域不可能满足 **strictly feasible**，那么就达不到强对偶性，于是 $p^\star\ned^\star$；2）问题可行域满足 **strictly feasible**，但是由于最优解达不到(比如 $\min 1/x$)，那么此时原问题和对偶问题仍满足强队偶性，但是原问题最优解达不到，而对偶问题则可以达到。

![LP duality](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-counter0.PNG)

![SDP/SOCP duality](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-counter1.PNG)

![SOCP duality](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-counter2.PNG)

![SDP duality](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/11-dual-counter3.PNG)

