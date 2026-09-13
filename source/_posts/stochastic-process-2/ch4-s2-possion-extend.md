---
title: 【随机过程2】泊松过程2 | 泊松过程扩展
date: 2021-11-09 10:03:08
tags:
 - 泊松过程
 - 更新过程
 - Wald等式
categories:
 - 随机过程2
mathjax: true
---

## 4.5 非时齐次泊松过程

**定义**：一个计数过程若满足：

1. $N(0)=0$；
2. 它是独立增量过程；
3. 对充分小的 $\Delta t>0$，有 $P(N(t+\Delta t)-N(t)=1) = \lambda(t) \Delta t + o(\Delta t)$，$P(N(t+\Delta t)-N(t)\ge 2)=o(\Delta t)$；

则称它为具有强度函数 $\{\lambda(t),t\ge0\}$ 的非时齐次泊松过程。

**定理 4.7**：若 $N(t),t\ge0$ 是非时齐次泊松过程，令 $m(t)=\int_0^t \lambda (s)ds$，则对 $\forall s,t\ge0$，有
$$
P(N(t+s)-N(s)=n) = \frac{(m(s+t)-m(s))^n}{n!}\exp(-(m(s+t)-m(s))), ~ n\ge0
$$
上述定理最主要特点在于 ${\mathbb E}[N(t+s)-N(s)] = m(s+t)-m(s) = \int_s^{s+t} \lambda (s)ds$，相应的方差为 $D_{[N(s+t)-N(s)]}=m(s+t)-m(s)$。



## 4.6 复合泊松过程

### 4.6.1 定义

**定义**：设 $\{Y_i,i\ge1\}$ 是独立同分布的随机变量序列，$\{N(t),t\ge0\}$ 为泊松过程，且与 $\{Y_i,i\ge1\}$ 独立，记 $X(t)=\sum_{i=1}^{N(t)} Y_i$ 称为复合泊松过程。

为求 $X(t)$ 的矩，先求它的矩母函数
$$
\begin{aligned}
\phi_t(u) &= {\mathbb E}[\exp(u X(t))] \\
&= \sum_{n=0}^\infty P(N(t)=n){\mathbb E}[\exp(uX(t)) | N(t)=n] \\
&= \exp(\lambda t(\phi_Y(u)-1))
\end{aligned}
$$
其中 $\phi_Y(u)={\mathbb E}[\exp(uY)]$ 为 $Y$ 的矩母函数。上式在 $u=0$ 处求导得到 ${\mathbb E}[X(t)] = \phi_t'(0) = \lambda t {\mathbb E}Y$，${D}[X(t)] = \phi_t''(0)-(\phi_t'(0))^2 = \lambda t{\mathbb E}Y^2$。若 $Y_i$ 取正整数的随机变量，则称 $\{X(t),t\ge0\}$ 为平稳无后效流。

### 4.6.2 复合泊松恒等式

**定理 4.8**：设 $Y=\sum_{i=1}^N X_i$ 是复合泊松随机变量，其中随机变量 $N$ 服从均值为 $\lambda$ 的泊松分布，随机变量序列 $\{X_k,k=1,2,...\}$ 是独立同分布的，且与 $N$ 统计独立。设 $X_k,(k=1,2,...)$ 的分布函数为 $F(x)$，则对任意的有界函数 $h(x)$ 有 ${\mathbb E}[Y h(Y)] = \lambda {\mathbb E}[X h(Y+X)]$，其中随机变量 $X$ 与 $N$ 统计独立，它的分布函数也为 $F(x)$。

证明：略。

**推论 4.8.1**：对任何正整数 $n$ 有 ${\mathbb E}[Y^n] = \lambda \sum_{k=0}^{n-1} \tbinom{n-1}{k} {\mathbb E}[Y^k]{\mathbb E}[X^{n-k}]$。

证明：令 $h(x)=x^{n-1}$ 即可得证。

利用此推论可以得到：
$$
\begin{aligned}
{\mathbb E}Y &= \lambda{\mathbb E}X \\
{\mathbb E}Y^2 &= \lambda{\mathbb E}X^2 + \lambda^2({\mathbb E}X)^2 \\
{\mathbb E}[Y-{\mathbb E}Y]^2 &= \lambda{\mathbb E}X^2 \\
{\mathbb E}[Y-{\mathbb E}Y]^3 &= \lambda{\mathbb E}X^3
\end{aligned}
$$


## 4.7 条件泊松过程

**定义**：设 $\Lambda$ 是一个正的随机变量，分布函数为 $G(x),x\ge0$，设 $\{N(t),t\ge0\}$ 是一个计数过程，且给定 $\Lambda=\lambda$ 的条件下，$\{N(t),t\ge0\}$ 是一个泊松过程，即 $\forall s,t\ge0,n\in \mathbb{N},\lambda\ge0$，有
$$
P(N(s+t)-N(s)=n | \Lambda=\lambda) = \frac{(\lambda t)^n}{n!} e^{-\lambda t}
$$
则 $\{N(t),t\ge0\}$ 是条件泊松过程。

**Remark**：这里 $\{N(t),t\ge0\}$ 本身并不是独立增量过程，由全概率公式得到
$$
P(N(s+t)-N(s)=n) = \int_0^{\infty} \frac{(\lambda t)^n}{n!} e^{-\lambda t} dG(\lambda)
$$
**定理 4.9**：设 $\{N(t),t\ge0\}$ 是上述条件泊松过程，则

1. ${\mathbb E}[N(s+t)-N(t)] = s{\mathbb E}\Lambda$
2. $D[N(s+t)-N(t)] = s{\mathbb E}\Lambda + s^2 D\Lambda$

证明：略。



## 4.8 更新过程

### 4.8.1 更新过程的定义

**定义**：设 $\{X_k,k\ge1\}$ 独立同分布的非负随机变量，分布函数为 $F(x)$，且 $F(0)<1$。令 $S_0=0, S_n=\sum_{k=1}^n X_k$，对 $\forall t\ge0$，记 $N(t)=\sup\{n:S_n\le t\}$ 或者 $N(t)=\sum_{n=1}^\infty I_{\{S_n\le t\}}$，称 $\{N(t),t\ge0\}$ 为更新过程。

记 $F_n(x)$ 为 $S_n$ 的分布函数，易知 $F_1(x)=F(x)$，$F_n(x)=\int_0^x F_{n-1}(x-u)dF(u),(n\ge2)$，即 $F_n(x)$ 是 $F(x)$ 的 $n$ 重卷积。记 $m(t)={\mathbb E}[N(t)]$，称 $m(t)$ 为更新函数。

类似的，分布密度函数同样是卷积的形式 $f_n(x) = \int_0^x f_{n-1}(x-u)f(u)du$。

**定理 4.10**：$\forall t\ge0$ 有 $m(t)=\sum_{n=1}^\infty F_n(t)$。

证明：$m(t)=\sum_n nP(N(t)=n) = \sum_n P(N(t)\ge n) = \sum_n P(S_n\le t) = \sum_n F_n(t)$.

*栗子 4.3*：$F(x)$ 是指数分布函数，相应的概率密度函数为 $f(x)=\lambda e^{-\lambda x},x\ge0,\lambda > 0$，那么由此可以计算 $f_n(x)=\frac{\lambda (\lambda x)^{n-1}}{(n-1)!} e^{-\lambda x}$，然后计算得到 $m(t) = \lambda t$，这与之前泊松过程的结论是一致的。

*栗子 4.4*：设 $F(x)$ 是 Gamma 分布函数，相应的额概率密度函数为 $f(x)=xe^{-x}$，其 **Laplace 变换**为 $\hat{f}(s) = 1/(1+s)^2$，利用 Laplace 变换的性质知道 $\hat{f_n}(s)=1/(1+s)^{2n}$，反变换即可得到 $f_n(x)$，然后再根据定理 4.10计算得到 $m(t)=-\frac{1}{4} + \frac{t}{2} + \frac{e^{-2t}}{4}$。



### 4.8.2 更新过程的剩余寿命与年龄

设 $N(t)$ 表示 $[0,t]$ 上事件发生的个数，$S_n$ 表示第 $n$ 个事件发生的时刻，那么 $S_{N(t)}$ 表示在 $t$ 之前最后一个事件发生的时刻，$S_{N(t)+1}$ 表示 $t$ 时刻后首次事件发生的时刻。令 $W(t)=S_{N(t)+1}-t, V(t)=t-S_{N(t)}$，则 $W(t)$ 表示 $t$ 时刻后直到首次事件发生的剩余时间。

**定理 4.11**：若非负随机变量 $\{X_n,n\ge1\}$ 独立同分布，分布函数为 $F(x)$，则对 $\forall x,t\ge0$，有

1. $P(W(t) > x)=1 - F(x+t) + \int_0^t P(W(t-u) > x) dF(u)$
2. $P(V(t) \le x) = (1-F(t)) I_{[0,x]}(t) + \int_0^t P(V(t-y) \le x) dF(y)$

证明：略。

**定理 4.12**：设 $\{N(t),t\ge0\}$ 是参数为 $\lambda$ 的泊松过程，则

1. $W(t)$ 与 $\{X_n,n\ge1\}$ 同分布，即 $P(W(t)\le x)=1-\exp(-\lambda x),x\ge0$
2. $V(t)$ 是截尾的指数分布，即 $P(V(t)\le x)=\begin{cases} 1-\exp(-\lambda x) & 0\le x < t \\ 1 & x\ge t \end{cases}$

证明：略。



## 4.10 瓦尔德等式

**定义**：设 $\{X_n,n\ge1\}$ 为随机序列，$T$ 为非负整数随机变量，若对任一 $n\in{\mathbb N}$ 事件 $\{T=n\}$ 仅依赖于 $\{X_1,...,X_n\}$，而与 $X_{n+1},X_{n+2},...$ 独立，则称 $T$ 关于 $\{X_n,n\ge1\}$ 是**停时**，或称马尔可夫时。

**定理 4.13（Wald）**：设 $\{X_n,n\ge1\}$ 独立同分布，$\mu={\mathbb E}X_n < \infty$，$X_n$ 与 $X$ 同分布，$\tau$ 关于 $\{X_n,n\ge1\}$ 是停时，且 ${\mathbb E}\tau < \infty$，则 ${\mathbb E}[\sum_{n=1}^\tau X_n] = {\mathbb E}X {\mathbb E}\tau$

证明：略。



## 4.11 泊松过程与鞅

线性方法构造的鞅，$Y(t)=N(t)-\lambda t, U(t)=Y^2(t)-\lambda t$

基于特征函数构造的鞅 $V(t)=\exp(-\theta N(t) + \lambda t(1-e^{-\theta}))$

