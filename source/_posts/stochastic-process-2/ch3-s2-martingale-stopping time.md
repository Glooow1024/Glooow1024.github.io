---
title:  【随机过程2】离散鞅论2 | 停时与停时定理
date: 2021-10-16 23:05:16
tags:
 - 鞅
 - 停时
 - 停时定理
 - 上穿不等式
 - 鞅收敛定理
 - 极大值不等式
 - Markov不等式
 - Azuma不等式
categories:
 - 随机过程2
mathjax: true
---

## 3.7 停时与停时定理

为了更好表述，下面给出一个不严谨的 $\sigma$ 域和停时的定义。

用 $F_n = \sigma(Y_k,0\le k \le n)$ 表示 $Y_0,...,Y_n$ 可提供的全部信息，称为他们生成的 $\sigma$ 域。

假设有非负随机变量 $\tau$ 和随机序列 $\{Y_n,n\ge0\}$，若 $\forall n\ge0$，$\{\tau=n\}\in F_n$，则称 $\tau$ 是 $\{Y_n,n\ge0\}$ 的**停时**。

<!--more-->

注：若 $\tau$ 是一个停时 $\forall n\ge0$，$\{\tau=n\}\in F_n$，那么可以导出事件 $\{\tau\le n\},\{\tau>n\},\{\tau\ge n\},\{\tau<n\}$ 均只由 $Y_0,...,Y_n$ 确定，这意味着截至到 $n$ 时刻，根据已有的信息可以完全确定停时所对应的事件是否已经发生。

停时的基本性质：设 $\tau,\sigma$ 是关于 $\{Y_n,n\ge0\}$ 的停时，则 $\tau+\sigma, \tau\wedge\sigma,\tau\vee\sigma$ 均是停时。

**定理 3.3（停时定理1）**：设 $\{X_n\}$ 是鞅，$\tau$ 是停时，若 

1. $P(\tau<\infty)=1$
2. ${\mathbb E}|X_\tau| < \infty$
3. $\lim_{n\to\infty} {\mathbb E}|X_n I_{\{\tau>n\}}| = 0$

则 ${\mathbb E}X_\tau = {\mathbb E}X_0$。

**定理 3.4（停时定理2）**：设 $\{X_n\}$ 是鞅，$\tau$ 是停时，若 

1. $P(\tau<\infty)=1$（也可以用其增强型条件 ${\mathbb E}\tau<\infty$）
2. ${\mathbb E}[\sup_{n\ge0} |X_{\tau \wedge n}|] < \infty$（这是定理 3.3 中后两个条件的增强型条件）

则 ${\mathbb E}X_\tau = {\mathbb E}X_{\tau \wedge n} = {\mathbb E}X_0$。

**推论 3.1（停时定理 3）**：设 $\{X_n\}$ 关于 $\{Y_n\}$ 是鞅，$\tau$ 是停时，且 ${\mathbb E}\tau<\infty$。若存在一个常数 $b<\infty$，满足对 $\forall n<\infty$ 有 ${\mathbb E}[|X_{n+1}-X_{n}| | Y_0,...,Y_n]\le b$，则 ${\mathbb E}X_\tau = {\mathbb E}X_0$。

**推论 3.2（停时定理 4）**：设 $\{X_n\}$ 是鞅，$\tau$ 是停时，若 

1. $P(\tau<\infty)=1$
2. $\forall n, {\mathbb E}[X_{\tau \wedge n}^2]$ 一致有界。

则 ${\mathbb E}X_\tau = {\mathbb E}X_0$。



在证明停时定理之前，需要先介绍两个引理。

**引理 3.1**：若 $\{X_n\}$ 是关于 $\{Y_n\}$ 的鞅，$\tau$ 是关于 $\{Y_n\}$ 的停时，则 $\forall n\ge1$ 有 ${\mathbb E}X_0 = {\mathbb E}X_{\tau\wedge n} = {\mathbb E}X_n$。

证明：${\mathbb E}X_{\tau\wedge n} = {\mathbb E}[X_\tau I_{\{\tau< n\}}] + {\mathbb E}[X_n I_{\{\tau \ge n\}}] = \sum_{k<n} {\mathbb E}[X_k I_{\{\tau=k\}}] + {\mathbb E}[X_n I_{\{\tau \ge n\}}]$，其中 
$$
{\mathbb E}[X_n I_{\{\tau = k\}}] = {\mathbb E}[{\mathbb E}[X_n I_{\{\tau=k\}} | Y_0,...,Y_k]] = {\mathbb E}[I_{\{\tau=k\}}{\mathbb E}[X_n | Y_0,...,Y_k]] = {\mathbb E}[X_k I_{\{\tau = k\}}]
$$
于是 ${\mathbb E}X_{\tau\wedge n} = {\mathbb E}X_n$。证毕。

**引理 3.2**：设 $X$ 是一个随机变量，满足 ${\mathbb E}|X|<\infty$，$\tau$ 是一个关于 $\{Y_n\}$ 的停时，且 $P(\tau<\infty)=1$，则 $\lim_{n\to\infty} {\mathbb E}[X I_{\{\tau>n\}}]=0,\lim_{n\to\infty} {\mathbb E}[X I_{\{\tau\le n\}}]={\mathbb E}X$。

证明：由于 $\lim_{n\to\infty} I_{\{\tau\le n\}}=1$，$\lim_{n\to\infty}{\mathbb E}[|X| I_{\{\tau\le n\}}] = {\mathbb E}|X|$，因此 $\lim_{n\to\infty} {\mathbb E}[|X| I_{\{\tau> n\}}]=0$，于是有 $\lim_{n\to\infty} {\mathbb E}[X I_{\{\tau> n\}}]=0$。证毕。



**证明（停时定理 1）**：暂略。



## 3.8 上穿不等式

对于给定区间 $(a,b),b>a$，如果一个随机序列先到达 $a$ 下面，再到达 $b$ 上面，即为上穿 $(a,b)$ 一次，上穿不等式就要研究序列上穿次数的问题。

对随机序列 $\{X_n\}$，令 $V^{(n)}(a,b)$ 是 $X_0,...,X_n$ 上穿 $(a,b)$ 的次数，令 $\alpha_0=0$，记 $\alpha_1$ 为首次到达 $(-\infty,a]$ 的时间，$\alpha_2$ 为 $\alpha_1$ 之后首次到达 $b$ 的时间，即 $\alpha_1 = \min\{n:n\ge0,X_n\le a\}$，$\alpha_2=\min\{n:n>\alpha_1,X_n\ge b\}$；依此类推 $\alpha_{2k-1}=\min\{n:n>\alpha_{2k-2},X_n\le a\}$，$\alpha_{2k}=\min\{n:n>\alpha_{2k-1},X_n\ge b\}$。于是可以定义上穿次数 $V^{(n)}(a,b)=\max\{k:k\ge0,\alpha_{2k}\le n\}$。

**定理 3.5（上穿不等式）**：设 $\{X_n\}$ 关于 $\{Y_n\}$ 是**下鞅**， $V^{(n)}(a,b)$ 是 $X_0,...,X_n$ 上穿 $(a,b)$ 的次数，则
$$
{\mathbb E}[V^{(n)}(a,b)] \le \frac{ {\mathbb E}[(X_n-a)^{+}] - {\mathbb E}[(X_0-a)^{+}]}{ b-a } \le \frac{ {\mathbb E}X_n^+ +|a|}{b-a}
$$
其中 $a^+=\max(a,0)$。

**证明**：因为 $\{X_n\}$ 关于 $\{Y_n\}$ 是下鞅，所以 $\{(X_n-a)^+\}$ 关于 $\{Y_n\}$ 也是下鞅。$\tilde{X}_n=(X_n-a)^+$ 上穿过 $(0,b-a)$ 的次数也是 $V^{(n)}(a,b)$，因此只需要证明 $(b-a){\mathbb E}[V^{(n)}(a,b)]\le {\mathbb E}\tilde{X}_n - {\mathbb E}\tilde{X}_0$。
$$
\begin{aligned}
{\mathbb E}\tilde{X}_n - {\mathbb E}\tilde{X}_0 &= {\mathbb E}\left[(\tilde{X}_{n}-\tilde{X}_{2V^{(n)}}) + \sum_{k=1}^{V^{(n)}}(\tilde{X}_{\alpha_{2k}} - \tilde{X}_{\alpha_{2k-1}}) + \sum_{k=1}^{V^{(n)}}(\tilde{X}_{\alpha_{2k-1}} - \tilde{X}_{\alpha_{2k-2}}) \right] \\
&= {\mathbb E}\left[(\tilde{X}_{n}-\tilde{X}_{2V^{(n)}})\right] + {\mathbb E}\left[\sum_{k=1}^{V^{(n)}}(\tilde{X}_{\alpha_{2k}} - \tilde{X}_{\alpha_{2k-1}})\right] + {\mathbb E}\left[\sum_{k=1}^{V^{(n)}}({\mathbb E}\tilde{X}_{\alpha_{2k}} - {\mathbb E}\tilde{X}_{\alpha_{2k-1}}) \right] \\
&= \text{I + II + III}
\end{aligned}
$$
根据下鞅的定义有 $\text{I,III}\ge0$，因此 $\text{II}\ge {\mathbb E}[(b-a)V^{(n)}(a,b)]$。证毕。

> **Remark**：为什么会有 $\text{III}>0$？上面的第二个等式成立吗？

**推论 3.5.1**：上鞅下穿不等式，略。

**定理 3.6（鞅收敛定理）**：设 $\{X_n\}$ 关于 $\{Y_n\}$ 是**下鞅**，$\sup_{n}{\mathbb E}|X_n|<\infty$，则存在一个随机变量 $X_{\infty}$ 使 $\{X_n,n\ge0\}$ 以概率 1 收敛于 $X_\infty$，即 $P(\lim_{n\to\infty} X_n=X_\infty)=1$，且 ${\mathbb E}|X_\infty|<\infty$。

证明：首先由于 ${\mathbb E}X_n^+ \le {\mathbb E}|X_n| \le 2{\mathbb E}X_n^+-{\mathbb E}X_n$，因此 $\sup_n {\mathbb E}|X_n|<\infty \iff \sup_n{\mathbb E} X_n^+<\infty$（存疑？）





## 3.9 极大值不等式与 Doob 定理

随机变量序列 $\{Y_n\}$ 独立同分布，且 ${\mathbb E}Y_n=0,{\mathbb E}Y_n^2=\sigma^2$，取 $X_0=0,X_n=\sum_{k\le n} Y_k$，对任意 $\varepsilon>0$，有

- **切比雪夫不等式**：$\varepsilon^2 P(|X_n|>\varepsilon)\le n\sigma^2$；
- **Kolmogorov不等式**：$\varepsilon^2 P(\max_{0\le k\le n} |X_k|>\varepsilon) \le n\sigma^2$。

**Markov不等式**：非负随机变量 $X$，对任意 $a>0$ 有 $aP(X\ge a)\le {\mathbb E}X$。

**Chernoff界**：随机变量 $X$，$\phi(t)={\mathbb E}[e^{tX}]$，对任意实数 $a>0$ 有

1. $t>0$ 时，有 $P(X\ge a)\le e^{-at}\phi(t)$；
2. $t<0$ 时，有 $P(X\le a)\le e^{-at}\phi(t)$。

注：${\mathbb E}[e^{tX}]=1+t{\mathbb E}X + \frac{t^2}{2!}{\mathbb E}X^2+\cdots$，当 $X$ 的各阶矩都知道的时候，特征函数也就知道了。

**引理 3.3**：$\{X_n\}$ 是下鞅，$\forall n\ge0,X_n\ge0$，则对任何 $\lambda>0$，有 $\lambda P(\max_{0\le k \le n} X_k>\lambda) \le {\mathbb E}X_n$。

**推论 3.3**：$\{X_n\}$ 是鞅，则对任意 $\lambda>0$ 有 

- $\lambda P(\max_{0\le k \le n} |X_k|>\lambda) \le {\mathbb E}|X_n|$
- $\lambda P(\max_{0\le k \le n} |X_k|^2>\lambda) \le {\mathbb E}|X_n|^2$

**定理 3.7（Doob）**：$\{X_k\}$ 是鞅，如果 $X_k\in L^p(P),1\le p<\infty$，则
$$
{\mathbb E}\left[\max_{1\le k\le n} |X_k|^p\right] \le \begin{cases}
q^p {\mathbb E}(|X_n|^p), & p>1,q=p/(p-1) \\
\frac{e}{e-1}\left(1+{\mathbb E}[|X_n|\log^+|X_n|] \right), & p=1
\end{cases}
$$
证明从略。



## 3.10 Azuma不等式

### 3.10.1 Azuma不等式

**引理 3.4**：若随机变量 $X$ 满足 ${\mathbb E}X=0$，$P(-\alpha \le X\le \beta)=1,\alpha>0,\beta>0$，则对任意的凸函数 $f(x)$ 有 ${\mathbb E}[f(X)]\le \frac{\beta}{\alpha+\beta}f(-\alpha)+\frac{\alpha}{\alpha+\beta}f(\beta)$。

**引理 3.5**：对任意参数 $k\in[0,1]$，有 $ke^{(1-k)x}+(1-k)e^{-kx}\le e^{x^2/8}$。

证明：略。

**定理 3.8（Azuma不等式）**：$\{Z_n,n\ge1\}$ 是鞅，$\mu={\mathbb E}Z_n$，令 $Z_0=\mu$，并假设存在非负常数 $\alpha_i,\beta_i,i\ge1$，满足条件 $-\alpha_i\le Z_i - Z_{i-1} \le \beta_i$，则对任意的 $n\ge1,a>0$ 有

1. $P(Z_n-\mu \ge a) \le \exp(-2a^2 / \sum_{i=1}^n (\alpha_i+\beta_i)^2)$
2. $P(Z_n-\mu \le -a) \le \exp(-2a^2 / \sum_{i=1}^n (\alpha_i+\beta_i)^2)$

证明：先假设 $\mu=0$，对任意的 $c>0$，则有 $P(\exp(cZ_n)\ge \exp(ca))\le {\mathbb E}[\exp(cZ_n)]\exp(-ca)$。令 $W_n=\exp(cZ_n)$，那么
$$
\begin{aligned}
{\mathbb E}[W_n | Z_{n-1}] &= \exp(cZ_{n-1}) {\mathbb E}[\exp(c(Z_n-Z_{n-1})) | Z_{n-1}] \\
&\le W_{n-1} \left[ \frac{\beta_n}{\alpha_n+\beta_n}\exp(-c\alpha_n) + \frac{\alpha_n}{\alpha_n+\beta_n}\exp(c\beta_n) \right]
\end{aligned}
$$
于是有 ${\mathbb E}W_n \le {\mathbb E}W_{n-1} (\beta_n\exp(-c\alpha_n) + \alpha_n\exp(c\beta_n)) / (\alpha_n+\beta_n)$，再利用引理 3.5 迭代即可证明 ${\mathbb E}W_n \le \exp(c^2 \sum_{i=1}^n(\alpha_i+\beta_i)^2/8)$。取 $c=4a/\sum_{i=1}^n(\alpha_i+\beta_i)^2$ 即可得证。第二个不等式可以用零均值的鞅 $\{Z_n-\mu\}$ 和 $\{\mu-Z_n\}$ 得到。

**推论 3.8.1**：如果向量 $X=(x_1,x_2,...,x_n)$ 和 $Y=(y_1,y_2,...,y_n)$ 最多只有一个坐标点不同，也就是说存在一个 $k$ 使得 $x_i=y_i,\forall i\ne k$。假设存在一个函数 $h(X)$ 满足条件 $|h(X)-h(Y)|\le 1$，假设 $X_1,...,X_n$ 为独立的随机变量，于是有 $P(h(X)-{\mathbb E}[h(X)] \ge a) \le \exp(-a^2/2n)$，$P(h(X)-{\mathbb E}[h(X)] \le -a) \le \exp(-a^2/2n)$。

证明：考虑（Doob）鞅 $Z_i = {\mathbb E}[h(X) | X_1,...,X_i], i=1,2,...,n$，那么有 
$$
\big|{\mathbb E}[h(X) | X_1=x_1,...,X_i=x_i] - {\mathbb E}[h(X) | X_1=x_1,...,X_{i-1}=x_{i-1}]\big| = \big|{\mathbb E}[h(x_1,...,x_i,X_{i+1},...,X_n)] - {\mathbb E}[h(x_1,...,x_{i-1},X_{i},...,X_n)]\big| \le 1
$$
因此 ${|Z_i-Z_{i-1}|} \le 1$，再取 $\alpha_i=-1,\beta_i=1$ 利用 Azuma 不等式即可。

### 3.10.2 Azuma不等式的推广

**引理 3.6**：假设 $Z_n$ 是一个零均值的鞅，$Z_0=0,-\alpha\le Z_i-Z_{i-1}\le\beta$，对所有的 $i>0$ 成立，于是有 $P(Z_n\ge a+bn) \le \exp(-8ab / (\alpha+\beta)^2)$，其中 $a,b>0$。

**证明**：类似前面 Azuma 不等式的证明，取 $W_n = \exp(c(Z_n-a-bn))$，那么可以验证 ${\mathbb E}[W_n | W_1,...,W_{n-1}]\le W_{n-1} \exp(-cb) \exp(c^2(\alpha+\beta)^2/8)$，取 $c=8b/(\alpha+\beta)^2$ 就可以得到 $\{W_n\}$ 为**上鞅**。对于一个固定的正整数 $k$，定义有界停时 $\tau=\min\{n:Z_n\ge a+bn, \text{ or } n=k\}$，于是有 $P(Z_{\tau} \ge a+b\tau) = P(W_\tau \ge 1) \le {\mathbb E}W_\tau \le {\mathbb E}W_0$，因此有 $P(Z_n\ge a+bn,n\le k) \le \exp(-8ab / (\alpha+\beta)^2)$。令 $k\to\infty$ 就得到所需结论，证毕。

**定理 3.9**：假设 $Z_n$ 是一个零均值的鞅，$Z_0=0,-\alpha\le Z_i-Z_{i-1}\le\beta$，对所有的 $i>0$ 成立，对任意的正常数 $c$ 和正整数 $m$ 有
$$
\begin{aligned}
P(Z_n\ge cn,n\ge m) &\le \exp(-2mc^2/(\alpha+\beta)^2) \\
P(Z_n\le -cn,n\ge m) &\le \exp(-2mc^2/(\alpha+\beta)^2)
\end{aligned}
$$
证明：如果存在一个 $n$ 满足条件 $n\ge m,Z_n\ge nc$，对这个 $n$ 有 $Z_n\ge nc\ge mc/2 + nc/2$，直接利用引理 3.6 即可得证。

> **Remark**：面对概率放缩问题，首先考虑 Markov 不等式，然后考虑 Chernoff 不等式，然后考虑 Azuma 不等式。

*栗子*：掷硬币，出现正面的概率为 $p$，抛掷 $m$ 次后出现正面的比率与 $p$ 相差大于 $\varepsilon$ 的概率是多少？

解：令 $S_n$ 为前 $n$ 次抛掷硬币中出现正面的次数，因此需要求解 $P(|S_n/n - p| > \varepsilon,n\ge m)$。取 $Z_n=S_n-np$，可以验证 $\{Z_n\}$ 为零均值的鞅，并且满足 $-p\le Z_n - Z_{n-1}\le 1-p$，利用推广的 Azuma 不等式就可以得到 $P(Z_n\ge \varepsilon n,n\ge m)\le \exp(-2m\varepsilon^2)$。

