---
title: 【随机过程2】连续参数马尔可夫链
date: 2022-01-24 17:28:25
tags:
  - Markov过程
  - 平稳分布
  - 极限分布
categories:
  - 随机过程2
mathjax: true
---

## 7.1 定义与基本概念

**定义**：设随机过程 $X=\{X(t),t\ge0\}$，状态空间 $S$，对任意 $0\le t_0 < t_1 < \cdots < t_n < t_{n+1}$，$i_k\in S$，若 $P(X(T_k)=i_k,0\le k\le n) > 0$ 有
$$
P(X(t_{n+1})=i_{n+1} | X(t_k)=i_k,0\le k\le n) = P(X(t_{n+1})=i_{n+1} | X(t_n)=i_n)
$$
则称 $X$ 为**连续参数的马氏链**。若对任意的 $s,t\ge0$，$i,j\in S$ 有
$$
P(X(s+t)=j | X(s)=i) = P(X(t)=j|X(0)=i) = P_{ij}(t)
$$
称 $X$ 为**齐次马氏链**。

对于连续参数马氏链，在原点处有 $P_{ij}(0)=\delta_{ij}$，因此 $P(0)=I$。除此之外假设其在原点处连续，即 $\lim_{t\to 0} P_{ij}(t)=\delta_{ij},\lim_{t\to0}P(t)=I$。类比离散参数的马氏链，可以得到类似的 C-K 方程 $P(s+t)=P(s)P(t)$。



## 7.2 转移概率矩阵

根据 C-K 方程可以猜测 $P(t)=e^{tQ}$，泰勒展开表示为 $P(t)=I + \sum_{n=1}^{\infty} \frac{t^n}{n!}Q^n$，$P(t)$ 完全由 $Q$ 确定，并且有 $P'(0)=\lim_{t\to0}\frac{P(t)-I}{t}=Q$。

上面只是猜测，那么是否真的存在这样一个 $Q$ 呢？下面两个定理给出结论。

**定理 7.1**：对 $i\in S$，极限 $-q_{ii}=\lim_{t\to0}\frac{1-P_{ii}(t)}{t}$ 存在，但可能是无穷。

**定理 7.2**：对 $i\in S$，极限 $q_{ij}=\lim_{t\to0}\frac{P_{ij}(t)}{t}$ 存在且有限。

证明：略。

**Remark**：实际应用中，$P(t)$ 很难获得，一般可以求得 $Q$，此时 $e^{tQ} = \lim_{n\to\infty}(I + Qt/n)^n$。如果取 $n=2^k$ 的形式，只需要 $k$ 次矩阵乘法即可。

**推论 7.1**：对任意 $i\in S$，$0\le \sum_{i\ne j}q_{ij}\le q_{ii}$。

证明：
$$
q_{ii}=\varliminf_{t\to0} \frac{1-P_{ii}(t)}{t} = \varliminf_{t\to0} \sum_{j\ne i}\frac{P_{ij}(t)}{t} \ge \sum_{j\ne i}\varliminf_{t\to0}\frac{P_{ij}(t)}{t} = \sum_{j\ne i}q_{ij}
$$
**推论 7.2**：当 $S$ 为有限状态空间时，$\sum_{i\ne j}q_{ij}= q_{ii} < \infty$。



## 7.3 Kolmogorov前向后向微分方程

**定义**：如果 $Q$ 满足 $\forall i\in S$，$\sum_{j\ne i}q_{ij} = q_{ii} < \infty$，则称 $Q$ 为保守矩阵。

**定理 7.3**：设马氏链 $X=\{X(t),t\ge0\}$，$Q=P'(0)$，当 $S$ 为**有限集**时，$P'(t)=P(t)Q=QP(t)$。

**Remark**：当 $S$ 为可数状态时，前向方程与后向方程不一定成立，根据 Fatu 引理有 $P'(t)\ge P(t)Q,P'(t)\ge QP(t)$

**定理 7.4**：当 $S$ 为**可列个状态**，$Q$ 为保守矩阵时，后向方程 $P'(t)=QP(t)$ 成立。



## 7.4 平稳分布与极限分布及其矩阵计算

类似离散马氏链，可以定义状态的连通关系。

**定义**：若 $\int_0^\infty P_{ii}(t)dt = +\infty$，则称状态 $i$ 为**常返态**，否则称为非常返态。

**定义**：设 $i$ 为常返态，若 $\lim_{t\to\infty}P_{ii}(t) > 0$，则称为正常返态，若 $\lim_{t\to\infty}P_{ii}(t) = 0$，称为零常返态。

**定义**：若概率分布 $\pi=\{\pi_i,i\in S\}$ 满足 $\pi=\pi P(t)$，称 $\pi$ 为 $X$ 的平稳分布。

**定理 7.5**：设 $X$ 是有限不可约连续马氏链，对任意 $i,j\in S$，有 $\lim_{t\to\infty}P_{ij}(t)=p_j$ 存在，且与状态 $i$ 无关。

证明：略。
