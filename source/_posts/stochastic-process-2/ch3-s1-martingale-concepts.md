---
title: 【随机过程2】离散鞅论1 | 基本概念
date: 2021-10-03 22:18:40
tags:
 - 条件期望
 - 鞅
 - Jensen不等式
 - 鞅分解定理
categories:
 - 随机过程2
mathjax: true
---

## 3.1 条件概率

在初等概率论中，条件期望的概念比较好理解，有 ${\mathbb E}[X | Y = y_j] = \sum_i x_i P(X=x_i | Y = y_j)$，得到的条件期望实际上是一个关于 $Y=y_j$ 的函数。但是在现代概率论中，这一概念进行了推广，也变得更加抽象，严谨的数学定义需要高等概率的知识，本教程基本只需要建立直观理解就够了。

<!--more-->

（不严谨地）简单来说，条件期望 ${\mathbb E}[X | Y]$ 是一个新的随机变量，也可以看作是随机变量 $Y$ 的函数。也就是可以理解为 $g(Y)$，这个函数由 $X,Y$ 的性质所决定。

性质（可以用直觉来理解这些性质，培养一下数学直观）：

- ${\mathbb E}[{\mathbb E}[X|Y]] = {\mathbb E}[X]$
- ${\mathbb E}[\sum_i a_i X_i | Y] = \sum_i a_i {\mathbb E}[X_i |Y]$
- ${\mathbb E}[g(X)h(Y) | Y] = h(Y) {\mathbb E}[g(X) | Y]$，$g(X),h(Y)$ 为有界函数
- ${\mathbb E}[X | X] = X$
- 若 $X,Y$ 独立，则 ${\mathbb E}[X|Y] = {\mathbb E}[X]$
- ${\mathbb E}[{\mathbb E}[X|Y,Z] ~|~ Y\in {\mathcal D}_j, Z\in{\mathcal D}_k] = {\mathbb E}[X | Y\in {\mathcal D}_j, Z\in{\mathcal D}_k]$
- ${\mathbb E}[{\mathbb E}[X|Y,Z] ~|~ Y] ={\mathbb E}[X|Y] = {\mathbb E}[{\mathbb E}[X|Y] ~|~ Y,Z]$



## 3.2 鞅的定义与基本性质

**定义**：$\forall n\ge0$，若 (1) ${\mathbb E} |X_n| < \infty$；(2) ${\mathbb E}[X_{n+1} | X_0,...,X_n]=X_n$，则过程 $\{X_n,n\ge0\}$ 称为鞅。

注：鞅过程和平稳过程没有相互包含关系，也不同于Markov过程。

有的时候 $X_n$ 不能直接观察，只能观察另一个过程 $\{Y_n,n\ge0\}$，因此将鞅的定义进行推广。

**定义（推广）**：设两个随即过程 $\{X_n,n\ge0\},\{Y_n,n\ge0\}$，若满足 (1) ${\mathbb E}|X_n|<\infty$；(2) ${\mathbb E}[X_{n+1} | Y_0,...,Y_n] = X_n$，则称 $\{X_n,n\ge0\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅。

性质：

- ${\mathbb E}[X_n | Y_0,...,Y_n] = X_n$，再推广可以有 ${\mathbb E}[X_{n-k} | Y_0,...,Y_n] = X_{n-k}, k\ge0$
- ${\mathbb E}[X_{n+k} | Y_0,...,Y_n] = X_{n+k}, k\ge0$
- ${\mathbb E}X_{n+1} = {\mathbb E}X_n = {\mathbb E}X_0$
- 若 $g(Y_0,...,Y_n)$ 有界，则 ${\mathbb E}[g(Y_0,...,Y_n)X_{n+k} | Y_0,...,Y_n] = g(Y_0,...,Y_n){\mathbb E}[X_{n+k} | Y_0,...,Y_n]$
- 若 $\{X_n,n\ge0\},\{Z_n,n\ge0\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅，则 $\{X_n\pm Z_n\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅
- 若 $\{X_n,n\ge0\},\{Z_n,n\ge0\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅，且 $X_n,Z_n$ 独立，则 $\{X_n Z_n\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅

注：${\mathbb E}[X_{n+1}X_n] = {\mathbb E}[{\mathbb E}[X_{n+1}X_n | Y_0,...,Y_n]] = {\mathbb E}[X_n {\mathbb E}[X_{n+1}|Y_0,...,Y_n]] = {\mathbb E}[X_n^2]$，说明鞅不是平稳过程。



## 3.3 鞅的举例与构造方法

*栗子 3.1*：$Y_0=0,\{Y_n,n\ge1\}$ 独立同分布，${\mathbb E}|Y_n|<\infty$，${\mathbb E}Y_n=0$，取 $X_0=0,X_n=\sum_{i=1}^nY_i$，则 $\{X_n,n\ge0\}$ 关于 $\{Y_n,n\ge0\}$ 是鞅。

*栗子 3.2*：$Y_0=0,\{Y_n,n\ge1\}$ 独立同分布，${\mathbb E}Y_n=0, {\mathbb E}Y_n^2=\sigma^2$，取 $X_0=0,X_n = \sum_{i=1}^n Y_i^2-n\sigma^2$，则 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

*栗子 3.3*（一般线性求和）：$X_n = \sum_{k=0}^n a_k(Y_0,...,Y_{k-1}) \cdot \{f(Z_k) - {\mathbb E}[f(Z_k)|Y_0,...,Y_{k-1}]\}$，要求 ${\mathbb E}|f(Z_k)|<\infty$，$|a_k(y_0,...,y_{k-1})|<A_k,\forall y_0,...,y_{k-1}$。（一般情况下取 $a_k = 1$）

*栗子 3.4*（Doob 鞅过程）：$X_n={\mathbb E}[X | Y_0,...,Y_n]$，$\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

*栗子 3.5*（似然比构成的鞅）：设 $\{Y_n\}$ 独立同分布，$f_0,f_1$ 是概率密度函数（PDF），$\forall y,f_0(y)>0$，令 $X_n=\frac{f_1(Y_0)\cdots f_1(Y_n)}{f_0(Y_0)\cdots f_0(Y_n)}$，当 $Y_n$ 的PDF为 $f_0$ 时，$\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

*栗子 3.6*：$\{Y_n\}$ 为 Markov 链，状态空间为 ${\mathcal S}$，转移矩阵为 $P$，定义 $F=(f(0),...,f(i),...)$ 为右特征向量，${\mathbb E}|f(Y_n)|<\infty$。令 $X_n = \lambda^{-n} f(Y_n)$，则 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

证明：${\mathbb E}|X_n| = \lambda^{-n} {\mathbb E}|f(Y_n)|<\infty$，
$$
{\mathbb E}[X_{n+1} | Y_0,...,Y_n] = {\mathbb E}[\lambda^{-n-1} f(Y_{n+1}) | Y_n] = \lambda^{-n-1} \sum_{j\in{\mathcal S}} f(y_j)P(Y_{n+1}=y_j|Y_n)=\lambda^{-n} f(Y_n)
$$
*栗子 3.7*（分支过程构造）：设 $Z^{(n)}(j)$ 表示第 $n$ 代第 $j$ 个个体产生的个体数目，$Z^{(n)}(i)(i=1,2,...)$ 独立同分布，${\mathbb E}[Z^{(n)}(i)]=m$，$Y_{n+1} = Z^{(n)}(1) + \cdots + Z^{(n)}(Y_n)$，则 $X_n=m^{-n}Y_n$ 关于 $\{Y_n\}$ 是鞅。${\mathbb E}[Y_{n+1} | Y_n] = Y_n {\mathbb E}[Z^{(n)}(1)] = mY_n$。

*栗子 3.8*（Wald鞅）：$Y_0=0,\{Y_n\}$ 独立同分布，$\phi(\lambda)={\mathbb E}[\exp(\lambda Y_n)]$，$X_0=1,X_n=\phi^{-n}(\lambda)\exp[\lambda (Y_1+\cdots+Y_n)]$，$\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

证明：定义 $S_n=\sum_{k=1}^n Y_k$，结合栗子 3.6 可知 $\{X_n\}$ 关于 $\{S_n\}$ 是鞅，由于 $S_1,...,S_n$ 与 $Y_1,...,Y_n$ 可以相互表示，因此 $\{X_n\}$ 关于 $\{Y_n\}$ 也是鞅。

*栗子 3.9*（R-N导数构成的鞅）：设 $Z\sim U[0,1]$，$f$ 是 $[0,1]$ 上的有界函数，令 $X_n=2^n [f(Y_n+2^{-n}) - f(Y_n)]$，而 $Y_n=\sum_{k=0}^{2^n-1} \frac{k}{2^n} {\mathbf 1}_{\{k/2^n \le Z < (k+1)/2^n\}}$，则 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。

证明：在 $Y_0,...,Y_n$ 条件下，$Z$ 服从 $[Y_n,Y_n+2^{-n})$ 的均匀分布，并且 $Y_{n+1}$ 以相同的概率等于 $Y_n$ 或者 $Y_n+2^{-n-1}$，于是有
$$
\begin{aligned}
{\mathbb E}[X_{n+1} | Y_0,...,Y_n] &= 2^{n+1} {\mathbb E}[f(Y_{n+1}+2^{-n-1})-f(Y_{n+1}) | Y_0,...,Y_n] \\
&= 2^{n+1}\left\{ \frac{1}{2} [f(Y_n+2^{-n-1})-f(Y_n)] + \frac{1}{2}[f(Y_n+2^{-n}) - f(Y_n+2^{-n-1})] \right\} \\
&= X_n
\end{aligned}
$$

> 小结（鞅的构造方法）：
>
> 1. 满足 Markov 性的序列，若满足 ${\mathbb E}[X_{n+1} | X_n] = \lambda X_n$，则 $Z_n = \lambda^{-n} X_n$ 构成一个鞅；
> 2. 满足 Markov 性的序列，若满足 ${\mathbb E}[g(X_{n+1}) | X_n] = f(\lambda) g(X_n)$，$g(x)$ 为有界函数，$f(\lambda)\ne0$，则 $Z_n = f(\lambda)^{-n} g(X_n)$ 构成一个鞅；
> 3. 一般线性求和。 



## 3.4 上鞅、下鞅的定义及基本性质

**定义**：随机过程 $\{X_n,n\ge0\},\{Y_n,n\ge0\}$ 满足下列条件：

1. ${\mathbb E}[X^-]>\infty, x^-:=\min(x,0)$；
2. ${\mathbb E}[X_{n+1} | Y_0,...,Y_n] \le X_n$；
3. $X_n$ 是 $Y_0,...,Y_n$ 的函数；

则称 $\{X_n\}$ 关于 $\{Y_n\}$ 是上鞅。同理可定义下鞅。

**性质**：

- 上鞅 $\{X_n\}$，则 ${\mathbb E}[X_{n+k} | Y_0,...,Y_n]\le X_n,\forall k\ge0$
- 上鞅 $\{X_n\}$，则 ${\mathbb E}[X_n] \le {\mathbb E}X_k \le {\mathbb E}X_0,0\le k\le n$
- 上鞅 $\{X_n\}$，$g(Y_0,...,Y_n)$ 是非负函数，则 ${\mathbb E}[g(Y_0,...,Y_n)X_{n+k} | Y_0,...,Y_n] = g(Y_0,...,Y_n) {\mathbb E}[X_{n+k} | Y_0,...,Y_n]$
- $\{X_n\},\{Z_n\}$ 关于 $\{Y_n\}$ 是上（下）鞅，则 $\{X_n+Z_n\}$ 关于 $\{Y_n\}$ 是上（下）鞅
- $\{X_n\},\{Z_n\}$ 关于 $\{Y_n\}$ 是上鞅，且 $X_n,Z_n$ 相互独立，则  $\{X_nZ_n\}$ 关于 $\{Y_n\}$ 是上鞅（应该要求 $X_n,Z_n\ge0$？？？）



## 3.5 Jensen不等式与下鞅的构造

**定理 3.1**：若 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅，$\varphi(x)$ 为一个凸函数，且对 $\forall n$，${\mathbb E}[\varphi(X_n)^+]<\infty$，则 $\{\varphi(X_n)\}$ 关于 $\{Y_n\}$ 是下鞅。

**推论 3.1.1**：若 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅，对 $\forall n$，${\mathbb E}[X_n^2]<\infty$，则 $\{|X_n|\},\{X_n^2\}$ 关于 $\{Y_n\}$ 是下鞅。

**推论 3.1.2**：若 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅，对 $\forall n,p\ge1$，${\mathbb E}[|X_n^p|]<\infty$，则 $\{|X_n|^p\}$ 关于 $\{Y_n\}$ 是下鞅。



## 3.6 分解定理

**定理 3.2（分解定理）**：对任意一个 $\{X_n\}$ 关于 $\{Y_n\}$ 的下鞅，必存在过程 $\{M_n\},\{Z_n\}$ 使得

1. $\{M_n\}$ 关于 $\{Y_n\}$ 是鞅
2. $Z_n$ 是 $Y_0,...,Y_n$ 的函数，且满足 $Z_1=0,Z_n\le Z_{n+1},{\mathbb E}Z_n<\infty$
3. $X_n=M_n+Z_n$

且上述分解是**唯一**的。

**证明**：证明的思路是首先构造出来这样一对 $M_n,Z_n$，然后证明他们确实满足条件，最后再证明唯一性。

构造 $Z_n = \sum_{k=1}^n{\mathbb E}[X_k -X_{k-1}| Y_0,...,Y_n]$，$M_n=X_n-Z_n$，可以验证 $\{M_n\}$ 是鞅并且 $Z_n$ 是 $Y_0,...,Y_n$ 的非负单调非降函数。唯一性证明用反证法。证毕。

**推论 3.2.1**：对任意一个 $\{X_n\}$ 关于 $\{Y_n\}$ 的上鞅，必存在过程 $\{M_n\},\{Z_n\}$ 使得

1. $\{M_n\}$ 关于 $\{Y_n\}$ 是鞅
2. $Z_n$ 是 $Y_0,...,Y_n$ 的函数，且满足 $Z_1=0,Z_n\le Z_{n+1},{\mathbb E}Z_n<\infty$
3. $X_n=M_n-Z_n$

且上述分解是**唯一**的。

