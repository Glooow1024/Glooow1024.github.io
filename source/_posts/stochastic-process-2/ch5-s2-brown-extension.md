---
title: 【随机过程2】布朗运动2 | 推广
date: 2022-01-24 17:28:21
tags:
 - 布朗运动
 - 首中时
 - 反正弦定理
 - 布朗桥
categories:
 - 随机过程2
mathjax: true
---

## 5.4 最大值与首中时的分布特性

设 $\{B(t),t\ge0\}$ 是标准布朗运动，不妨设 $B(0)=0$。

**定义**：首次击中 $a$ 的时间 $\tau_a=\inf\{t:t\ge0,B(t)=a\}$

<!--more-->

**定义**：对 $\forall t > 0, M(t)=\max_{0\le u\le t}B(u)$ 表示 $[0,t]$ 上的最大值。

当 $a > 0$ 时，显然存在等价关系 $\{\tau_a \le t\} = \{M(t) \ge a\}$，因此有 $P(\tau_a \le t) = P(M(t) \ge a)$。

**Remark**：事实上，这与泊松过程中定义的 $\{S_n\le t\} = \{N(t)\ge n\}$ 类似。主要差别在于泊松过程中讨论离散点情况，这里讨论连续情况。

**定理 5.6**：对任意 $a>0$，$M(t)$ 和 $\tau_a$ 的分布密度函数分别为
$$
\begin{aligned}
f_{M(t)}(a) &= \sqrt{\frac{2}{\pi t} } e^{-a^2/ 2t} I_{[0,\infty)}(a) \\
f_{\tau_a}(t) &= \frac{a}{\sqrt{2\pi} } e^{-a^2/2t} t^{-3/2}, \quad t > 0
\end{aligned}
$$
$\tau_a$ 的 Laplace 变换为 ${\mathbb E}[\exp(-s\tau_a)] = e^{-\sqrt{2s}a}, ~ s>0$。

证明：$P(B(t)\ge a) = P(B(t\ge a | \tau_a \le t))P(\tau_a \le t)$，由于 $P(B(t)\ge a) | \tau_a\le t) = P(B(t)< a) | \tau_a\le t)=1/2$，因此可以得到 $P(M(t)\ge a) = P(\tau_a \le t) = 2P(B(t)\ge a)=\frac{2}{\sqrt{2\pi t} }\int_a^{\infty} e^{-x^2 / 2t}dx$。于是 $f_{M(t)}(a)={d(1-P(M(t)\ge a))} / {da}$，$f_{\tau_a}(t) = d(P(\tau_a \le t)) / da$，证毕。

利用上述定理可以到如下结果：

1. $\tau_a$ 几乎处处有限，即 $P(\tau_a < \infty)=1$；
2. ${\mathbb E}\tau_a = \infty$。

证明：1）$P(\tau_a<\infty) = \lim_{t\to\infty}=P(\tau_a \le t) = \lim_{t\to\infty} \frac{2}{\sqrt{2\pi} }\int_{a/\sqrt{t} }^{\infty} e^{-u^2/2}du = 1$；2）${\mathbb E}[\exp(-s\tau_a)]=e^{-\sqrt{2s}a}$，两边对 $s$ 求导得到 ${\mathbb E}[\tau_a \exp(-s\tau_a)] = \frac{\sqrt{2} }{2} a s^{-1/2} e^{-\sqrt{2s}a}$，令 $s\to 0$ 即可得到 ${\mathbb E}\tau_a = \infty$，证毕。

*栗子 5.1*：



**定理 5.7**：设 $\{B(t)\}$ 为标准布朗运动，$\forall a > 0, y\ge0$ 有 $P(B(t)\le a-y, M(t)\ge a) = P(B(t) \ge a+y)$

证明：根据反射性定义 $B^{\ast}(t) = 2a-B(t)$，则有 $\tau_a = \tau^{\ast}_a = \inf\{B^{\ast}(t)=a\}$，又由于 $\{M(t)\ge a\} = \{\tau_a \le t\}$，于是有
$$
\begin{aligned}
P(B(t)\le a-y, M(t)\ge a) &= P(B(t)\le a-y, \tau_a\le t) \\
&= P(B^{\ast}(t) \le a-y, \tau_a \le t) \\
&= P(B(t) \ge a+y, \tau_a\le t) \\
&= P(B(t)\ge a+y)
\end{aligned}
$$
**推论 5.7.1**：设 $\{B(t)\}$ 为标准布朗运动，$\forall a > 0$ 有 $P(M(t)\ge a) = 2P(B(t)\ge a) = P(|B(t)|\ge a)$.

**推论 5.7.2**：$\forall \xi > 0, x\le\xi$，$M(t)$ 和 $B(t)$ 的联合分布密度函数为 $f_{M,B}(\xi,x) = \sqrt{\frac{2}{\pi} }(\frac{2\xi-x}{t^{3/2} }) \exp(-(2\xi-x)^2/2t) I_{[0,\infty)}(\xi)I_{(-\infty,\xi]}(x)$.



## 5.5 过零点的反正弦定理

$\forall t_1 < t_2$，记事件 $o(t_1,t_2)=\{\exists t\in(t_1,t_2), B(t)=0\}$，利用全概率公式有
$$
P(o(t_1,t_2)) = \int_{-\infty}^{\infty} P(o(t_1,t_2) | B(t_1)=x) \frac{1}{\sqrt{2\pi t_1} }e^{-x^2/2t_1} dx
$$
利用布朗运动的连续性和对称性有
$$
P(o(t_1,t_2)|B(t_1)=x) = P(\tau_x\le t_2-t_1) = \frac{2}{\sqrt{2\pi(t_2-t_1)} }\int_x^{\infty} e^{-u^2/2(t_2-t_1)} du
$$
于是有
$$
P(o(t_1,t_2)) = \frac{1}{\pi\sqrt{t_1(t_2-t_1)} }\int_0^{\infty}\int_x^{\infty} e^{-\frac{y^2}{2(t_2-t_1)} }dy \cdot e^{-\frac{x^2}{2t_1} }dx
$$
这个积分的直接求解非常困难，为求此积分，采用另外的方法，也即反正弦定理。

**定理 5.8（反正弦定理）**：设 $B(t)$ 是标准布朗运动，记 $\bar{o}(t_1,t_2)=\{B(t)\ne 0,\forall t\in(t_1,t_2)\}$，则 $P(\bar{o}(t_1,t_2))=\frac{2}{\pi} \arcsin\sqrt{\frac{t_1}{t_2} }$，且当 $t_1=xt, t_2=t, 0 < x < 1$ 时，有 $P(\bar{o}(xt,t))=\frac{2}{\pi} \arcsin \sqrt{x}$.

**Remark**：这说明布朗运动处处连续但处处不可导。



## 5.6 布朗运动的推广

### 5.6.1 带吸收点的布朗运动

设 $Z(t)=\begin{cases}B(t), & t < \tau_x \\ x, & t\ge \tau_x \end{cases}$，为求 $Z(t)$ 的分布，不妨设 $x > 0$

（1）当 $y > x$ 时，$P(Z(t) \le y)=1$；

（2）当 $y=x$ 时，$P(Z(t)=x) = P(\tau_x\le t) = \frac{2}{\sqrt{2\pi t} }\int_x^\infty e^{-u^2/2t} du$；

（3）当 $y < x$ 时，



### 5.6.2 原点反射的布朗运动



### 5.6.3 几何布朗运动

令 $W(t)=e^{B(t)}$，称它为几何布朗运动，取 $B(t)$ 的矩母函数 $\phi(s)={\mathbb E}[e^{sB(t)}]$，则 $\phi(s)=e^{ts^2/2}$，因此 ${\mathbb E}[W(t)]=\phi(1)=e^{t/2}$，$D[W(t)] = {\mathbb E}[W^2(t)]-({\mathbb E}[W(t)])^2=\phi(2)-e^t = e^{2t}-e^t$.

*栗子 5.2*：考虑股票市场收益率，$S_0,S_n$ 分别表示最初价位和最终价位，$S_n=S_0 e^{B(t_1)-B(t_0)}\cdots e^{B(t_n)-B(t_{n-1})}$.



### 5.6.4 布朗运动的积分

令 $S(t)=\int_0^t B(u)du$，根据正态分布的性质知道他是正态过程，${\mathbb E}S(t)=0$，$\forall 0\le \delta \le t$，有
$$
\operatorname{cov}[S(\delta),S(t)] = {\mathbb E}[\int_0^\delta\int_0^t B(u)B(v)dudv] = \int_0^\delta\int_0^t(u\wedge v)dudv=\frac{\delta^2}{2}(t-\frac{\delta}{3})
$$


### 5.6.5 布朗运动的形式导数

考虑增量比，固定 $\Delta t > 0$，令 $\frac{B(t+\Delta t)-B(t)}{\Delta t} = \frac{\Delta B(t)}{\Delta t}$，





## 5.7 布朗桥与经验分布

### 5.7.1 布朗桥的基本概念与性质

**定义**：$\{B(t),t\ge0\}$ 为标准布朗运动，不妨设 $B(0)=0$，令 $B_{00}(t)=B(t)-tB(1)$，则称 $\{B_{00}(t), 0\le t\le 1\}$ 为**布朗桥**。

根据其定义，可以得到基本性质如下：

1. $B_{00}(0)=B_{00}(1)=0$；（？？）
2. ${\mathbb E}[B_{00}(t)]={\mathbb E}[B(t)-tB(1)]=0$；
3. $D[B_{00}(t)]={\mathbb E}[B_{00}^2(t)]=t(1-t)$；
4. 设 $s\le t$，$\operatorname{cov}[B_{00}(s),B_{00}(t)]=s(1-t)$；
5. $\{B_{00}(t),0\le t\le1 \}$ 的分布与 $\{B(t), t\ge0 \}$ 在 $B(1)=0$ 下的条件分布相同。



### 5.7.2 经验分布与布朗桥的关系

工程统计中，经常独立抽取多个样本 $X_1,...,X_n$ 来统计某参量的统计特性，定义经验分布 $\hat{F}_n(x) = \frac{1}{n}\sum_{i=1}^n I_{\{X_i\le x\} }$。假设 $X_1,...,X_n$ 独立同分布，且 $X_n\sim F(x)$，则有：

1. ${\mathbb E}[\hat{F}_n(x)] = F(x)$
2. $\operatorname{Var}[\hat{F}_n(x)]=\frac{1}{n}F(x)(1-F(x))$
3. 任意给定 $x$，利用强大数定理得到 任意给定 $x$，利用强大数定理得到$P(\lim_{n\to\infty} \hat{F}_n(x)=F(x))=1$
4. 任意给定 $x$，利用中心极限定理得到 $n\to\infty$ 时有（依分布收敛） 

$$
\frac{\sum_{i=1}^n (I_{\{X_i\le x\} }-F(x)) }{\sqrt{nF(x)(1-F(x))} } \overset{d}{=} {\mathcal N}(0,1)
$$

证明：（2）$\operatorname{Var}[\hat{F}_n(x)]={\mathbb E}[\hat{F}_n(x)^2] - F(x)^2 = \frac{1}{n^2}{\mathbb E}[\sum_{i=1}^n I_{\{X_i\le x\} } \sum_{j=1}^n I_{\{X_j\le x\} } ]$，展开化简就可以得到性质 2.

（4）记 $G_n(x)=\sqrt{n}(\hat{F}_n(x)-F(x))$，容易证明 ${\mathbb E}G_n(x)=0,\operatorname{Var}G_n(x)=F(x)(1-F(x))$，特别的，根据中心极限定理，当 $n\to\infty$ 时有 $G_n(x)\overset{d}{=}G(x)$，其中 $G(x)\sim {\mathcal N}(0,F(x)(1-F(x)))$。

**引理 5.1**：如果 $x_1 < x_2$，则 ${\mathbb E}[G_n(x_1)G_n(x_2)] = F(x_1)(1-F(x_2))$。

证明：代入展开化简即可得到。

> **Remark**：根据上述引理和 $G_n(x)$ 的性质可知，当 $F_n(x)=x$ 时，即 $X_1,...,X_n$ 服从 $[0,1]$ 上的均匀分布时，$G_n(x)$ 的极限分别 $G(x)$ 是布朗桥。
>
> 事实上，对于一般的连续分布函数 $F(x)$，$G_n(x)$ 的极限分布 $G(x)$ 也可以用布朗桥表示。为此，首先给出如下引理。

**引理 5.2**：随机变量 $X$ 的分布函数 $F(x)$ 连续，则 $Y=F(X)$ 是一个 $[0,1]$ 均匀分布的随机变量。

证明：$\forall y\in[0,1]$，定义 $x=\inf\{u:F(u)\ge y\}$，因此有 $P(F(X)\ge y)) = P(Y\ge y)=1-F(x)=1-y$，从而有 $P(Y < y) = y$，证毕。

基于引理 5.2，可以定义 $G^{ \# }(x)=B_{00}(F(x))$，利用布朗桥性质有 $G^{ \# }(x)$ 与 $G(x)$ 有相同的分布特性，从而可以推得 $G_n(x)$ 的极限分布为以 $F(x)$ 为参变量的布朗桥。



### 5.7.3 经验分布的误差估计

略。





## 5.8 带漂移的布朗运动

**定义**：设 $\{B(t),t\ge0\}$ 为标准布朗运动，记 $X(t)=B(t)+\eta t$，其中 $\eta$ 为常数，称 $\{X(t),t\ge0\}$ 为带漂移的布朗运动。

### 5.8.1 移出区间的概率计算

**定理 5.9**：对任意 $A > 0, B > 0$，定义停时 $\tau=\inf\{t:X(t)=A ~ or ~ X(t)=-B \}$，则 $P_{A}=P(X(\tau)=A) = \frac{e^{2\eta B}-1}{e^{2\eta B}-e^{2\eta a} }$.



### 5.8.2 首中时问题





## 5.9 布朗运动的轨道性质

**轨道处处连续，几乎处处不可导**。

**定理**：标准布朗运动 $B(t)$，对任意的 $t\ge0$，有
$$
P(\lim_{h\to 0}\sup\left|\frac{B(t+h)-B(t)}{h}\right|=+\infty) = 1
$$
证明：略。

