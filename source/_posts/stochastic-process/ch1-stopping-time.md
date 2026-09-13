---
title: 【随机过程（数学系）】基本概念
date: 2021-06-04 22:19:05
tags:
  - 样本轨道
  - 循序可测
  - 可分性
  - 停时
  - 首中时
categories:
  - 随机过程2
mathjax: true
---

第一章主要介绍随机过程中的基本概念。

## 1. 基本概念

**什么是随机过程？**

**定义**：设 $(\Omega, \mathcal{F},P)$ 是一个概率空间，$(E,\mathcal{E})$ 是一个可测空间，$\mathcal{T}$ 是一个指标集，$X_{\mathcal{T} }=\{X_t,t\in{\mathcal T}\}$ 是一族定义在 $(\Omega, \mathcal{F},P)$ 上，取值于 $(E,\mathcal{E})$ 中的随机变量，称 $X_T$ 为（E-值）**随机过程**。对每个 $\omega\in\Omega$，$X_T(\omega)=\{X_t(\omega),t\in T\}$ 称为随机过程的一个**实现**，或者**样本轨道**，或者**样本函数**。

> **注**：随机过程实际上是一个二元函数 $X(t,\omega)$，因此这里的 $\omega$ 所属的样本空间 $\Omega$ 包含了所有时刻 ${\mathcal T}$ 的所有可能事件，而不是只针对某个时刻随机变量 $X_t$ 的样本空间。

<!--more-->

**定义**：设 $X_T$ 是一个 $E$-值随机过程，对任意 $\Lambda=\{t_1,\cdots,t_n\}\subset T$，称 $\mu_\Lambda(B_\Lambda) = P((X_{t_1},\cdots,X_{t_n})\in B_\Lambda), B_\Lambda\in{\mathcal E}^\Lambda$ 为 $X_T$ 的一个**有限维分布**。称 $\{\mu_\Lambda, \Lambda\Subset T\}$ 为 $X_T$ 的**有限维分布族**。

> **注**：这里相当于是取了 $X_T$ 在一个下标集子集（一般取至多可数集）上的限制，这么做一半是为了从连续时间随机过程转到离散时间，可能便于处理。

**命题 1.1.1**：$\{\mu_\Lambda,\Lambda\Subset T\}$ 是**相容**的，也就是说对于 $\Lambda_1\subset\Lambda_2\Subset T$，有 $\mu_{\Lambda_2}(B_{\Lambda_1}\times E^{\Lambda_2\backslash \Lambda_1})=\mu_{\Lambda_2}(B_{\Lambda_1}).$

证明：略。

**定义**：设 $X_T,Y_T$ 是两个 $E$ 值随机过程

1. 若 $X_T,Y_T$ 有相同的有限维分布族，则称 $Y_T$ 是 $X_T$ 的一个**版本**；
2. 若 $P(X_t=Y_t)=1,\forall t\in T$，则称 $Y_T$ 是 $X_T$ 的一个**修正**；
3. 若 $P(X_t=Y_t,\forall t\in T)=1$，则称 $Y_T$ 是 $X_T$ 的一个**不可区别**；

> **注**：$(3)\Rightarrow(2)\Rightarrow(1)$。



## 2. 可测性

**定义**：设 $X_T$ 是定义在 $(\Omega, \mathcal{F},P)$ 上取值于 $(E,\mathcal{E})$ 中的随机过程，若 $X(t,\omega):([0,+\infty)\times\Omega, {\mathcal B}_{[0,+\infty)}\times{\mathcal F}) \to (E,{\mathcal E})$ 是可测的，则称过程 $X_T$ 是**可测**的。

**定义**：设 $\{ {\mathcal F}_t,t\in T\}(T\subset {\mathbb R}_+ \})$ 是 ${\mathcal F}$ 的一族单调上升子 $\sigma$ 代数（即 $s\le t\rightarrow {\mathcal F}_s\subset {\mathcal F}_t $，此时也称 $\{ {\mathcal F}_t,t\in T\}$ 为 ${\mathcal F}$ 的一个子 $\sigma$ 域流），$X_T$ 是 $E$ 值随机过程，称 $X_T$ 是 $\{ {\mathcal F}_t,t\in T\}$ **适应**的，若 $\forall t\in T, X_t:(\Omega,{\mathcal F}_t)\to(E,{\mathcal E})$ 可测（简单来说即 $X_t$ 是 ${\mathcal F}_t$ 可测的）。若 $\forall t\in T,X(s,\omega):([0,t)\times\Omega, {\mathcal B}_{[0,t)}\times{\mathcal F}_t) \to (E,{\mathcal E})$ 可测，则称 $X_T$ **循序可测**。

**命题 1.2.1**：设 $X_T=\{X_t,t\ge0\}$ 是实值随机过程，关于域流 $\{ {\mathcal F}_t, t\ge0\}$ 适应，若 $X_T$ 是右连续的，则它一定是循序可测的。



## 3.  可分性

**定义**：设 $X_T=\{X_t,t\ge0\}$ 是一个实值随机过程，$Q\subset [0,+\infty)$ 可数稠密，若 $\forall \omega$，$\{(t,X_t(\omega)), t\ge0\}\subset\{(r,X_r(\omega)), r\in Q \}^-$（$A^-$ 表示 $A$ 的闭包），则称 $X_T$ 关于 $Q$ 可分。$Q$ 称为一个可分集，若存在可数稠密 $Q\subset [0,+\infty)$ 使 $X_T$ 关于 $Q$ 可分，则称 $X_T$ 可分。

> **注**：可分性用的比较多的大概是把原来的连续指标集 $T$ 转化成可数指标集 $Q$，便于证明。

**命题 1.3.1**：若实值随机过程 $X_T=\{X_t,t\ge0\}$ 可分，则 $\forall c$，$\{X_t\ge c, t\ge0\}\in {\mathcal F}$。

证明：利用可分性，将其表示为可数个集合的交集。

**定理 1.3.2**：任一实值随机过程必有可分修正。



## 4. 停时

**定义**：设 $(\Omega, \mathcal{F},P)$ 是一个概率空间，$\{ {\mathcal F}_t,t\in T\}$ 是 ${\mathcal F}$ 的一个上升子 $\sigma$ 域流，$T\subset {\mathbb R}_+$ 是一个时间参数集，$\tau$ 是从 $\Omega$ 到 $T$ 中的映射，若 $\{\tau\le t\}\in{\mathcal F}_t,\forall t\in T$，则称 $\tau$ （关于 $\{ {\mathcal F}_t,t\in T\}$）是一个**停时**。若 $\{\tau< t\}\in{\mathcal F}_t,\forall t\in T$，则称 $\tau$ （关于 $\{ {\mathcal F}_t,t\in T\}$）是一个**宽停时**。

> **注**：引用网上对于停时的直观解释：$\{\tau\le t\}\in{\mathcal F}_t$ 意味着到 $t$ 时刻的信息足够判断是否在 $t$ 时刻或 $t$ 之前停下来。

**性质 1.4.1**：

1. 若 $\tau,\sigma$ 是两个停时，则 $\tau \vee\sigma, \tau\wedge\sigma,\tau+\sigma$ 也是停时；
2. 若 $\{\tau_n,n\ge1\}$ 是一列宽停时，则 $\sup_n \tau_n,\inf_n \tau_n, \liminf \tau_n,\limsup \tau_n$ 也是宽停时；
3. 若 $\{\tau_n,n\ge1\}$ 是一列停时，则 $\sup_n \tau_n$ 也是停时；
4. $\tau$ 是 $\{ {\mathcal F}_t,t\ge0\}$ 停时 $\iff$ $\tau$ 关于域流 ${\mathcal F}_{t+}\triangleq \cap_{s>t}{\mathcal F}_s,t\in T$ 是停时；
5. 若 $\tau$ 是宽停时，则它是一列停时，$\tau_n$ 的递减极限，即 $\tau_n \downarrow \tau$；
6. 若 $T={\mathbb Z_+}\text{ or }{\mathbb N}$（可数），则 $\tau$ 是停时 $\iff$ $\{\tau=t\}\in {\mathcal F}_t,\forall t\in T$。

证明：略

**定义**：设 $\tau$ 是 ${\mathcal F}_t$ 停时，定义 ${\mathcal F}_\tau=\{A\in{\mathcal F}, A\cap \{\tau\le t\}\in {\mathcal F}_t,\forall t\in T \}$，则 ${\mathcal F}_\tau$ 是停时。

**定理**：设 $\tau$ 是 ${\mathcal F}_t$ 停时，$X_t$ 是 ${\mathcal F}_t$ 适应的，则

1. $\tau$ 是 ${\mathcal F}_\tau$ 可测的；
2. 若 $T={\mathbb Z}_+\text{ or }{\mathbb N}$，则 $X_\tau$ 是 ${\mathcal F}_\tau$ 可测的；
3. 若 $T=[0,+\infty)$ 且 $\{X_t,t\ge0\}$ 是关于 $\{ {\mathcal F}_t,t\ge0\}$ 循序可测，则 $X_\tau$ 是 ${\mathcal F}_\tau$ 可测的。

证明：略。



接下来定义某个时间首次发生的时间作为首中时，实际上很多情况下首中时就是一种停时，他在很多问题中都会出现，比如赌博前决定赢1毛钱就收手，首次发生“赢1毛钱”这个事件的时刻就是一个停时。

**定义**：$\{X_t,t\ge0\},(E,{\mathcal E}),B\in{\mathcal E}$，定义**首中时** $\tau_B=\inf\{t:X_t(\omega)\in B\}$

**定理 1.4.2**：

1. 若 $T={\mathbb Z}_+$，则 $\forall B\in{\mathcal E}$，$\tau_B$ 是停时；
2. 若 $(E,{\mathcal E})$ 是距离可测空间，$T={\mathbb R}_+$，$X_t$ 适应， $B$ 是**开集**，若 $X_t$ 是关于 $t$ **右连续**的，则 $\tau_B$ 是**宽停时**；
3. 若 $(E,{\mathcal E})$ 是距离可测空间，$T={\mathbb R}_+$，$X_t$ 适应， $B$ 是**闭集**，若 $X_t$ 是关于 $t$ **连续**的，则 $\tau_B$ 是**停时**。

证明：略。

> **注1**：若 $\{ {\mathcal F}_t\}$ 右连续（即 ${\mathcal F}_{t+}={\mathcal F}_t,t\ge0$）则$B$无论开、闭，$\tau_B$ 都是停时。
>
> **注2**：这里为什么会有开集/闭集的区别？是怎么跟停时/宽停时的定义联系在一起的？以及为什么会对 $X_t$ 的连续性有要求？

> **注**：实际上我们常用的域流是自然 $\sigma$ 域流，即 ${\mathcal F}^X_t=\sigma\{X_s:s\le t\}$。

