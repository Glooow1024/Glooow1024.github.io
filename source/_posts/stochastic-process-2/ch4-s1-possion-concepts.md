---
title: 【随机过程2】泊松过程1 | 定义与基本性质
date: 2021-11-02 08:49:28
tags:
 - 泊松过程
 - 指数分布
 - 顺序统计量
categories:
 - 随机过程2
mathjax: true
---

## 4.1 泊松过程的定义与基本性质

**定义 4.1**：随机过程 $\{N(t),t\ge0\}$ 称为时齐泊松过程，若满足下列条件：

1. 他是一个计数过程，且 $N(0)=0$；
2. （独立增量）任取 $0 < t_1 < t_2 < \cdots < t_n$，$N(t_1),N(t_2)-N(t_1),...,N(t_n)-N(t_{n-1})$ 相互独立；
3. （平稳增量）$\forall s,t\ge0,n\ge0$，$P(N(s+t)-N(s)=n) = P(N(t)=n)$；
4. 对任意 $t>0$ 和充分小的 $\Delta t>0$，有 $P(N(t+\Delta t)-N(t)=1) = \lambda \Delta t + o(\Delta t)$，$P(N(t+\Delta t)-N(t)\ge 2)=o(\Delta t)$；

其中 $\lambda>0$ 称为强度常数，$o(\Delta t)$ 为高阶无穷小。

<!--more-->

**定义 4.2**：计数过程 $\{N(t),t\ge0\}$，被称为参数为 $\lambda$ 的时齐泊松过程，若满足如下条件：

1. $N(0)=0$；
2. 它是独立增量过程；
3. $\forall s,t\ge0,N(s+t)-N(s)$ 是参数为 $\lambda t$ 的泊松分布，即 $P(N(t+s)-N(s)=k) = \frac{(\lambda t)^k}{k!} e^{-\lambda t}$。

> 两种定义是等价的。

基本性质：

1. 均值 ${\mathbb E}[N(t)] = \lambda t$；
2. 方差 $\text{var}(N(t)) = {\mathbb E}[(N(t)-\lambda t)^2] = \lambda t$；
3. 特征函数 $\phi_{N(t)}(x) = {\mathbb E}[\exp(-j N(t)x)] = \exp(-\lambda t e^{jx})$；
4. ${\mathbb E}[N(t)^2] = (\lambda t)^2 + \lambda t$；
5. 自相关函数 $R(t+\tau,t) = {\mathbb E}[N(t+\tau)N(t)] = (\lambda t)^2 + \lambda t + \lambda^2 t\tau,(\tau>0)$（非平稳过程）

*栗子 4.1*：$\{N(t), t\ge0\}$ 是参数为 $\lambda$ 的时齐泊松过程，$S_0=0,S_n$ 为第 $n$ 个事件发生的时刻，则 $N(t)$ 关于 $\{S_n,n\ge0\}$ 不是停时，但是 $N(t)+1$ 关于 $\{S_n,n\ge0\}$ 是停时。

证明：$\{N(t)=n\} \iff \{S_n\le t < S_{n+1} \}=\{S_n\le t \} - \{S_{n+1}\le t \}$，因此 $\{N(t)=n\}$ 可以由 $\{S_0,...,S_{n+1} \}$ 构成的事件表示，因此 $N(t)+1$ 关于 $\{S_n,n\ge0\}$ 是停时。



## 4.2 泊松过程与指数分布的关系

$N(t),t\ge0$ 是计数过程，令 $S_0=0,S_n$ 表示第 $n$ 个事件发生的时刻，$X_n=S_n-S_{n-1}$ 表示第 $n$ 个与第 $n-1$ 个事件之间的间隔，于是有 $S_n=\inf\{t:N(t)=n \},n\ge1$。相应的 $N(t)$ 可以表示为 $N(t)=\sum_{n=1}^{\infty} I_{[0,t]}(S_n)$。

$P(S_n \le t) = P(N(t)\ge n) = 1-e^{-\lambda t} \sum_{k=0}^{n-1} \frac{(\lambda t)^k}{k!}$，特别当 $n=1$ 时，有 $P(S_1\le t) = P(X_1\le t) = 1-e^{-\lambda t}$，即 $X_1\sim E(\lambda)$ 是参数为 $\lambda$ 的指数分布。同样的可以得到 $X_n(n\ge2)$ 也服从指数分布，均值为 $1/\lambda$，方差为 $1/\lambda^2$。

**定理 4.1**：计数过程是泊松过程的**充要条件**是 $\{X_n,n\ge1\}$ 是独立的同**指数分布**。

证明：略。



## 4.3 到达时间的条件分布

### 4.3.1 到达时间的条件分布

**定理 4.2**：设 $N(t),t\ge0$ 是泊松过程，则对 $\forall 0< s < t$ 有 $P(X_1\le s | N(t)=1) = s/t$。

证明：$P(X_1\le s | N(t)=1) = P(X_1\le s, N(t)=1) / P(N(t)=1) = P(N(s)=1, N(t)-N(s)=0) / P(N(t)=1)$。

**定理 4.3**：设 $N(t),t\ge0$ 是泊松过程，则对 $\forall 0< s < t,k\le n$ 有 $P(S_k \le s | N(t)=n) = \sum_{l=k}^n \frac{n!}{l!(n-l)!}(\frac{s}{t})^l (1-\frac{s}{t})^{n-l}$。特别当 $k=n$ 时，有 $P(S_n\le s | N(t)=n) = (\frac{s}{t})^n$。

证明：略。

### 4.3.2 顺序统计量

$Y_1,...,Y_n$ 是 $n$ 个随机变量，如果 $Y_{(k)}$ 是 $Y_1,...,Y_n$ 中第 $k$ 个最小的随机变量，我们称 $Y_{(1)},...,Y_{(n)}$ 是关于 $Y_1,...,Y_n$ 的顺序统计量。如果 $Y_1,...,Y_n$ 是独立同分布的连续随机变量，其概率密度分布为 $f(y)$，则 $Y_{(1)},...,Y_{(n)}$ 的联合分布概率密度函数为
$$
f(y_1,...,y_n) = n! \Pi_{k=1}^n f(y_k), ~ y_1 < y_2 < \cdots < y_n
$$
特别的，当 $Y_1,...,Y_n$ 为 $(0,t)$ 上独立的均匀分布随机变量时，相应的顺序统计量 $Y_{(1)},...,Y_{(n)}$ 的联合分布概率密度函数为
$$
f(y_1,...,y_n) = n! / t^n, ~ 0< y_1 < y_2 < \cdots < y_n < t
$$
**定理 4.4**：设 $N(t),t\ge0$ 为泊松过程，则在已给 $N(t)=n$ 时事件相继发生的时间 $S_1,...,S_n$ 的条件概率密度为
$$
f(t_1,t_2,...,t_n) = \begin{cases}n!/t^n, & 0< t_1 < t_2 < \cdots < t_n \\ 0, & others \end{cases}
$$
证明：对任取的 $0=t_0 < t_1 < t_2 < \cdots < t_n < t_{n+1}=t$，取 $h_0=h_{n+1}=0$ 及充分小的 $h_i$，则 
$$
\begin{aligned}
&P(t_i < S_i \le t_i+h_i, 1\le i\le n | N(t)=n) \\
=& \frac{P(N(t_i+h_i)-N(t_i)=1,1\le i\le n, ~ N(t_{j+1})-N(t_j+h_j)=0, 1\le j\le n)}{P(N(t)=n)} \\
=& \frac{n!}{t^n} h_1 h_2 \cdots h_n 
\end{aligned}
$$
取极限即可得证。

> **Remark**：上述定理表明，若非负函数 $g(x_1,...,x_n)$ 是关于 $x_i, i=1,...,n$ 的对称函数，即对任意一种排列模式 $\phi$，有 $g(x_1,...,x_n) = g(x_{\phi(1)}, x_{\phi(2)},..., x_{\phi(n)})$（也就是函数值与各分量的顺序无关），则在概率分布的意义上下列等式成立
> $$
> g(S_1,...,S_n|N(t)=n) \overset{d}{=} g(Y_{(1)},...,Y_{(n)}) = g(Y_1,...,Y_n)
> $$
> 其中 $Y_1,...,Y_n$ 为 $(0,t)$ 上独立的均匀分布随机变量。

*栗子 4.2*：设某工地有一工程任务，工人到达工地遵照参数为 $\lambda$ 的泊松流，求在时刻 $t$ 工人完成的总的工程量的期望值。

解：设第 $i$ 个工人到达工地的时刻为 $S_i$，在 $[0,t]$ 内工人完成的总工程量为 $S(t)=\sum_{i=1}^{N(t)}(t-S_i)$。有 ${\mathbb E}[S(t) | N(t)=n] = nt - {\mathbb E}[\sum_{i=1}^n S_i | N(t)=n]=nt - {\mathbb E}[\sum_{j=1}^n Y_j | N(t)=n]=nt/2$，于是 ${\mathbb E}[S(t)] = {\mathbb E}[{\mathbb E}[S(t) | N(t)=n]] = \lambda t^2/2$.



**定理 4.5**：设 $N(t),t\ge0$ 是参数为 $\lambda$ 的泊松过程，$S_k,k\ge1$ 为到达时刻，则对任意的 $[0,+\infty)$ 上可积函数 $f$ 有 ${\mathbb E}[\sum_{n=1}^{\infty} f(S_n)] = \lambda\int_0^\infty f(s)ds$.

证明：当 $t\ge0$ 时，有 $\{S_n\le t\}=\{N(t)\ge n\}$，因此有 $P(S_n\le t)=P(N(t)\ge n) = \sum_{j=n}^\infty \frac{(\lambda t)^j}{j!} e^{-\lambda t}$，求导得到 $S_n$ 的概率密度为 $f_{S_n}(t)=\lambda \frac{(\lambda t)^{n-1}}{(n-1)!} e^{-\lambda t}I_{\{t\ge0\}}$，因此 ${\mathbb E}[f(S_n)] = \lambda \int_0^\infty f(s)\frac{(\lambda s)^{n-1}}{(n-1)!} e^{-\lambda s} ds$，再对 $n$ 求和即可得证。

*栗子 4.3*：题干同上面的栗子 4.2.

解：定义 $f(s)=I_{[0,t]}(s)(t-s)$，则 $S(t)=\sum_{i=1}^{\infty} f(s_i)$，利用定理 4.5 结论即可得到 ${\mathbb E}[S(t)] = \lambda t^2 /2$。



## 4.4 泊松过程的分流

**定理 4.6**：设 $N(t),t\ge0$ 是参数为 $\lambda$ 的泊松过程，到达事件的类型取决于它到达的时间。如果某到达时间是 $s>0$，则它属于类型 1 的概率为 $P(s)$，输于类型 2 的概率为 $1-P(s)$。假设 $N_m(t),(m=1,2)$ 表示 $(0,t]$ 内到达的类型 $m$ 的事件数，则 $N_1(t)$ 和 $N_2(t)$ 是两个独立的泊松变量，相应的均值分别为 $\lambda pt$ 和 $\lambda(1-p)t$，其中 $p=\frac{1}{t}\int_0^t P(s) ds$。

证明：$P(N_1(t)=k, N_2(t)=l) = P(N_1(t)=k, N_2(t)=l | N(t)=k+l) P(N(t)=k+l)$，考虑发生在 $(0,t]$ 的事件，如果事件在时刻 $s$ 发生，由于其在 $(0,t]$ 服从均匀分布，那么该事件是类型 1 的概率为 $p=\frac{1}{t}\int_0^t P(s)ds$。

另外由于这些事件相互独立，因此 $P(N_1(t)=k, N_2(t)=l | N(t)=k+l)=\tbinom{k+l}{k}p^k(1-p)^l$。证毕。

> **Remark**：如果 $P(s)$ 与 $s$ 无关，那么分流之后将得到两个新的泊松流，否则不是泊松流。