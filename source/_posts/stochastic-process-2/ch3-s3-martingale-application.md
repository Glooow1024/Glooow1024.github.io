---
title: 【随机过程2】离散鞅论3 | 鞅论应用
date: 2021-10-16 23:08:38
tags:
 - 鞅
 - 停时定理
categories:
 - 随机过程2
mathjax: true
---

## 3.11 鞅论的应用

*栗子 1*：三人赌博，每一轮从中随机依次选出两个人，第一个被选中的人给第二个一枚硬币。如果其中一个人没有硬币，其余两人继续赌博，直到其中一个人赢得所有硬币。假设三个人被选中概率完全相等，且每次选择相互独立。如果最初三人拥有的硬币数分别为 $a,b,c$，求此游戏结束的平均时间。

<!--more-->

**解**：设三个人在第 $n$ 次赌博之后拥有的硬币分别为 $X_n,Y_n,Z_n$，记 $S_n=X_n Y_n+Y_n Z_n+X_n Z_n$。定义 $M_n=\sum_{k=1}^n (S_k - {\mathbb E}[S_k | X_0,Y_0,Z_0,...,X_{k-1},Y_{k-1},Z_{k-1}])$，则可以证明 $\{M_n\}$ 是鞅。下面想要计算 $M_n$ 的具体表达式，需要分情况讨论：

- case1：$X_{k-1}Y_{k-1}Z_{k-1}>0$，可以验证 ${\mathbb E}[X_k Y_k | X_{k-1}=x,Y_{k-1}=y] = xy-1/3$，因此有 ${\mathbb E}[S_k | X_{k-1},Y_{k-1},Z_{k-1}] = S_{k-1}-1$；
- case2：$X_{k-1}=0$，此时有 $X_k=0$，${\mathbb E}[S_k | X_{k-1},Y_{k-1},Z_{k-1}] = S_{k-1}-1$；

因此有 $M_{n} = S_n-S_0+n$，可以验证**鞅的停时定理 3** 条件成立，从而有 ${\mathbb E}[M_{\tau}] = {\mathbb E}[M_0]=0$，又有 ${\mathbb E}[M_\tau] = {\mathbb E}S_\tau - {\mathbb E}S_0 + {\mathbb E}\tau = {\mathbb E}\tau - {\mathbb E}S_0 = 0$，因此 ${\mathbb E}\tau={\mathbb E}S_0=ab+bc+ac$。

> **Note**：如何想到这样定义一个鞅？首先考虑一阶统计量 $S_n=X_n+Y_n+Z_n$ 恒等于常数，因此考虑二阶统计量。

*栗子 2*（随机徘徊）：一维随机徘徊，$Y_n$表示第 $n$ 个时刻质点移动的距离，$P(Y_n=1)=P(Y_n=-1)=1/2$，在 $n$ 时刻质点离开原点的距离为 $X_n=\sum_{i=1}^n Y_i$，其中 $X_0=0$，容易验证 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅。下面需要讨论：

1. 从原点到达 1 的平均时间；
2. 设 $a<0<b$ 为两个整数，质点先到达 $a$ 的概率 $P_a$？
3. 设 $a<0<b$ 为两个整数，质点到达 $a$ 或 $b$ 的平均时间？

*解*：(1) 定义停时 $\tau_1=\min \{n: X_0=0,X_n=1 \}$，那么需要求 ${\mathbb E}\tau$，可以考虑用**鞅的停时定理 3**，首**先假设条件 ${\mathbb E}\tau<\infty$ 成立**，${\mathbb E}[|X_{n+1}-X_n| | Y_0,...,Y_n]={\mathbb E}[|Y_{n+1}|]=1$，于是停时定理 3 条件满足，那么就有 ${\mathbb E}X_{\tau} = {\mathbb E}X_0=0$，但是根据停时的定义有 ${\mathbb E}X_\tau=1$，矛盾，这说明假设 ${\mathbb E}\tau<\infty$ 不成立，所以 ${\mathbb E}\tau=\infty$。

(2) 设 ${\tau}$ 表示从 $0$ 出发达到 $a$ 或 $b$ 的时间，$\tau$ 关于 $\{Y_n\}$ 是停时，容易验证 $P(\tau<\infty | X_0=0)=1$，而且 $|X_{\tau \wedge n}| \le \max{-a,b}$，因此 ${\mathbb E}[\sup_{n} |X_{\tau\wedge n}|]<\infty$，**鞅的停时定理 2** 条件满足，因此有 ${\mathbb E}X_\tau = aP_a + bP_b ={\mathbb E}X_0=0$，另外有 $P_a+P_b=1$，于是可以得到 $P_a=b/(b-a)$。

(3) 设 $\tau$ 为从 $0$ 出发达到 $a$ 或 $b$ 的时间，$\tau$ 关于 $\{Y_n\}$ 是停时，接下来的关键就是构造一个新的鞅，使其与时间 $n$ 产生直接的关系，这样利用停时定理的时候就能求得 ${\mathbb E}\tau$。那么可以取 ${Z_n} = \sum_{k=1}^n (X_k^2 - {\mathbb E}[X_k^2 | Y_0,...,Y_{k-1}])$，关于 $\{Y_n \}$ 是鞅，并且可以验证 ${\mathbb E}[X_k^2 | Y_0,...,Y_{k-1}]={\mathbb E}[(X_{k-1}+Y_k)^2 | Y_0,...,Y_{k-1}] = X_{k-1}^2+1$，因此有 ${Z_n}=X_n^2-n$。验证停时定理 3 的条件 ${\mathbb E}[|Z_{n+1}-Z_n| | Y_0,...,Y_n] \le 2|X_n|{\mathbb E}|Y_{n+1}| + 1+1 \le 2\max(-a,b)+2$，根据停时定理 3 有 ${\mathbb E}Z_\tau = P_a(a^2-{\mathbb E}\tau) + P_b(b^2 - {\mathbb E}\tau) = {\mathbb E}Z_0=0$，故 ${\mathbb E}\tau=-ab$。

**Note**：利用停时定理时，条件 ${\mathbb E}\tau<\infty$ 可以先假设成立，然后用求解的结果来验证。



## 3.12 连续鞅论

**定义（鞅）**：$\{X(t), t\ge0\}$ 为一个随机过程，如果 $\{X(t),t\ge0\}$ 满足下列条件：

1. （存在性）对任意的 $t\ge0$，有 ${\mathbb E}|X(t)|<\infty$；
2. （鞅性）对任意的 $0\le t_0<t_1<\cdots<t_n<t_{n+1}$，有 ${\mathbb E}[X(t_{n+1}) | X(t_1),...,X(t_n)] = X(t_{n})$；

则称 $\{X(t),t\ge0\}$ 是**鞅**。

**定义（上鞅）**：$\{X(t), t\ge0\}$ 为一个随机过程，如果 $\{X(t),t\ge0\}$ 满足下列条件：

1. （存在性）对任意的 $t\ge0$，有 ${\mathbb E}X(t)^-<\infty$；
2. （鞅性）对任意的 $0\le t_0<t_1<\cdots<t_n<t_{n+1}$，有 ${\mathbb E}[X(t_{n+1}) | X(t_1),...,X(t_n)] \le X(t_{n})$；

则称 $\{X(t),t\ge0\}$ 是**上鞅**。下鞅定义类似。

**定理 3.12（停时定理）**：设 $\{X(t),t\ge0\}$ 是鞅，$\tau$ 是关于 $\{X(t),t\ge0\}$ 的停时，若 $P(\tau<\infty)=1$，且 ${\mathbb E}[\sup_{t\ge0} |X_{\tau \wedge t}|] < \infty$，则 ${\mathbb E}X_\tau = {\mathbb E}[X(0)]$。

