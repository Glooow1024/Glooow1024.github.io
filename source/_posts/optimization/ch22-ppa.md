---
title: 凸优化笔记22：近似点算法
date: 2020-05-09 14:04:09
tags:
  - 近似点算子
  - 增广拉格朗日函数
  - PPA
  - ALM
categories:
  - Convex Optimization
mathjax: true
---

在进入具体的优化算法后，我们首先讲了基于梯度的，比如梯度下降(GD)、次梯度下降(SD)；然后又讲了近似点算子，之后讲了基于近似点算子的方法，比如近似点梯度下降(PG)、对偶问题的近似点梯度下降(DPG)、加速近似点梯度下降(APG)。而这一节讲的，还是基于近似点的！他叫**近似点方法(Proximal Point Algorithm, PPA)**，除此之外还会介绍**增广拉格朗日方法(Augmentted Larangian Method, ALM)**。我们就开始吧！

<!--more-->

## 1. 近似点方法

近似点方法跟近似点梯度下降很像，在此之外我们先简单回顾一下 PG 方法。对优化问题
$$
\text{minimize } f(x)=g(x)+h(x)
$$
其中 $g$ 为光滑凸函数，而且为了保证收敛性需要满足 Lipschitz 光滑性质，$h$ 为非光滑函数，只要 $h$ 为闭凸函数，对于近似点算子 $\text{prox}_{h}(x)$ 自然满足firmly nonexpansive(co-coercivite) 性质，这个也等价于 Lipschitz continuous 性质
$$
\left(\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right)^{T}(x-y) \geq\left\|\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right\|_{2}^{2}
$$
迭代格式为
$$
x_{k+1}=\text{prox}_{th}(x_k-t_k\nabla g(x_k))
$$
这个表达式实际上可以等价表示为
$$
x^+ = x-tG_t(x), \qquad G_t(x):=\frac{x-x^+}{t}\in \partial h(x^+)+\nabla g(x)
$$
然后我们再回顾一下 APG 方法，实际上就是在 PG 的基础上引入了一个外差，直观理解就是加入了动量
$$
\begin{aligned}
x_{k+1} &= \text{prox}_{th}(y_k-t_k\nabla g(y_k)) \\
y_k &= x_k + w_k(x_k-x_{k-1})
\end{aligned}
$$
好了复习结束！那么近似点方法 PPA 针对的优化问题是 $\min f$，其中 $f$ 为闭凸函数

> **迭代格式**为
> $$
> \begin{aligned}
> x_{k+1} &= \text{prox}_{t_k f}(x_k) \\
> &= \arg\min_u \left( f(u)+\frac{1}{2t_k}\Vert u-x_k\Vert_2^2 \right)
> \end{aligned}
> $$

这实际上可以看作是 PG 方法中取函数 $g=0$，因此所有适用于 PG 的收敛性分析也都适用于 PPA 方法，而且由于 $g=0$，因此也不需要对 $f$ 做 Lipschitz 光滑的假设，因此**步长 $t_k$ 可以是任意正实数，而不需要 $0< t_k < 1/L$**。类比 PG 中的收敛性分析可以得到
$$
t_{i}\left(f\left(x_{i+1}\right)-f^{\star}\right) \leq \frac{1}{2}\left(\left\|x_{i}-x^{\star}\right\|_{2}^{2}-\left\|x_{i+1}-x^{\star}\right\|_{2}^{2}\right) \\

\Longrightarrow f\left(x_{k}\right)-f^{\star} \leq \frac{\left\|x_{0}-x^{\star}\right\|_{2}^{2}}{2 \sum_{i=0}^{k-1} t_{i}} \quad \text { for } k \geq 1
$$
同样得，我们也可以引入外差进行加速
$$
x_{k+1}=\operatorname{prox}_{t_{k} f}\left(x_{k}+\theta_{k}\left(\frac{1}{\theta_{k-1}}-1\right)\left(x_{k}-x_{k-1}\right)\right) \quad \text { for } k \geq 1 
$$
其中可以是任意 $t_k> 0$，$\theta_k$ 由以下方程解得
$$
\frac{\theta_{k}^{2}}{t_{k}}=\left(1-\theta_{k}\right) \frac{\theta_{k-1}^{2}}{t_{k-1}}
$$
并且可以证明加速后的方法收敛速度可以达到 $O(1/k^2)$。

PPA 的基本原理就没有了，这里简单总结一下， 实际上核心的地方只有一个迭代格式
$$
x_{k+1} = \text{prox}_{t_k f}(x_k)
$$
其他的收敛性分析以及加速算法都可以类比 PG 得到。

## 2. 增广拉格朗日方法

增广拉格朗日方法(也叫乘子法)一般是为了解决有约束优化问题，并且我们通常考虑等式约束，对于非等式约束可以通过引入松弛变量将其转化为等式约束。这里我们首先介绍一下基本的 ALM 形式。对于优化问题
$$
\begin{aligned}
\min\quad& f(x) \\
\text{s.t.}\quad& C(x)=0
\end{aligned}
$$
**增广拉格朗日函数(重要)**为
$$
L_\sigma(x,\nu) = f(x)+\nu^TC(x) + \frac{\sigma}{2}\Vert C(x)\Vert_2^2,\quad \sigma>0
$$
就是在初始的拉格朗日函数后面加了一个等式约束的二次正则项

> **ALM 的迭代格式**则为
> $$
> \begin{aligned}
> x^{k+1} &= \arg\min_{x} L_\sigma(x,\nu^k) \\
> \nu^{k+1} &= \nu^k + \sigma C(x^{k+1})
> \end{aligned}
> $$

一般会将增广拉格朗日函数化简成另一种形式(**重要**)
$$
L_\sigma(x,\nu) = f(x) + \frac{\sigma}{2}\Vert C(x)+\frac{\nu}{\sigma}\Vert_2^2
$$
就是做了一个配方，但化简前后的两个函数并不完全等价，因为丢掉了 $\nu$ 的二次项，不过对于迭代算法没有影响，因为迭代的第一步仅仅是针对 $x$ 求最小。

如果是不等式约束呢？比如优化问题
$$
\begin{aligned}
\min_x\quad& f(x) \\
\text{s.t.}\quad& C(x)\ge0
\end{aligned}
\iff
\begin{aligned}
\min_{x,s}\quad& f(x) \\
\text{s.t.}\quad& C(x)-s=0,\quad s\ge0
\end{aligned}
$$
此时增广拉格朗日函数为
$$
L_\sigma(x,s,\nu) = f(x)-\nu^T(C(x)-s)+\frac{\sigma}{2}\Vert C(x)-s\Vert^2,\quad s\ge0
$$
迭代方程为
$$
\begin{aligned}
(x^{k+1},s^{k+1}) &= \arg\min_{x,s\ge0} L_\sigma(x,s,\nu^k) \\
\nu^{k+1} &= \nu^k - \sigma (C(x^{k+1})-s^{k+1})
\end{aligned}
$$
第一步求极小要怎么计算呢？先把增广拉格朗日函数化为
$$
\min_x\left\{f(x)+\min_{s\ge0}\frac{\sigma}{2}\left\Vert C(x)-s-\frac{\nu}{\sigma}\right\Vert^2 \right\} \\
= \min_x\left\{f(x)+\frac{\sigma}{2}\left\Vert C(x)-\frac{\nu}{\sigma}-\Pi_+(C(x)-\frac{\nu}{\sigma})\right\Vert^2 \right\}
$$
其中 $\Pi_+$ 表示向 $R_+^n$ 空间的投影。

***例子***：这是一个应用 ALM 的例子，考虑优化问题 $\min f(x),\text{ s.t. }Ax\in C$，用 ALM 的迭代步骤为
$$
\begin{aligned}
\hat{x} &= \arg\min_{x} f(x)+\frac{t}{2}d( Ax+z/t)^2 \\
z :&= z + t(A\hat{x}-P_C(A\hat{x}+z/t))
\end{aligned}
$$
其中 $P_C$ 是向集合 $C$ 的投影，$d(u)$ 是 $u$ 到集合 $C$ 的欧氏距离。

## 3. PPA 与 ALM 的关系

这里先给出一个结论：**对原始问题应用 ALM 等价于对对偶问题应用 PPA**。

下面看分析，考虑优化问题
$$
\begin{aligned}
(P)\text { minimize }\quad& f(x)+g(A x)\\
(D)\text { maximize } \quad& -g^{\star}(z)-f^{\star}\left(-A^{T} z\right)
\end{aligned}
$$
我们就先来看看原始问题应用 ALM 会得到什么。原始问题等价于
$$
\begin{aligned}
\min\quad& f(x)+g(y) \\
\text{s.t.}\quad& Ax=y
\end{aligned}
$$
拉格朗日函数为
$$
L_t(x,y,z) = f(x)+g(y)+z^T(Ax-y)+\frac{t}{2}\Vert Ax-y\Vert^2
$$
ALM 迭代方程为
$$
\begin{aligned}
(x^{k+1},y^{k+1}) &= \arg\min_{x,y} L_t(x,y,z^k) \\
z^{k+1} &= z^k + t(Ax^{k+1}-y^{k+1})
\end{aligned}
$$
对偶问题应用 PPA 的迭代方程是什么呢？首先我们令 $h(z)=g^{\star}(z)+f^{\star}\left(-A^{T} z\right)$，那么就需要求解
$$
z^{+} = \text{prox}_{th}(z) = \underset{u}{\operatorname{argmin}}\left(f^{\star}\left(-A^{T} u\right)+g^{\star}(u)+\frac{1}{2 t}\|u-z\|_{2}^{2}\right) \\

\iff z-z^+ \in t\partial h(z^+)=t\left(-A\partial f^\star(-A^Tz^+)+\partial g^\star(z^+\right)
$$
这个 $z^+$ 乍一看跟 ALM 的 $z^{k+1}$ 没有一点关系啊，为什么说他们俩等价呢？这就要引出下面一个等式了（先打个预防针，这个等式以及他的推导很不直观，我也没有想到一个很好的解释，但是这个等式以及推导又很重要！在后面章节中也会用到）

> 很重要的式子：
> $$
> z^+=\text{prox}_{th}(z) = z+t(A\hat{x}-\hat{y})
> $$
> 其中 $\hat{x},\hat{y}$ 为
> $$
> (\hat{x}, \hat{y})=\underset{x, y}{\operatorname{argmin}}\left(f(x)+g(y)+z^{T}(A x-y)+\frac{t}{2}\|A x-y\|_{2}^{2}\right)
> $$

先不管推导，这样看来对偶问题的 PPA 是不是就跟原始问题的 ALM 完全等价了呢？！然后我们来看一下证明（更多的是验证上面这两个等式成立，至于怎么推导出来我也不知道......）

增广拉格朗日函数可以转化为
$$
(\hat{x},\hat{y}) = \underset{x, y}{\operatorname{argmin}}\left(f(x)+g(y)+\frac{t}{2}\|A x-y+z/t\|_{2}^{2}\right)
$$
我们把它表示成一个优化问题，并且引入等式约束
$$
\begin{aligned}
\text{minimize}_{x, y, w} \quad& f(x)+g(y)+\frac{t}{2}\|w\|_{2}^{2} \\
\text{subject to}\quad& A x-y+z / t=w
\end{aligned}
$$
他的 KKT 条件就是
$$
A x-y+\frac{1}{t} z=w, \quad-A^{T} u \in \partial f(x), \quad u \in \partial g(y), \quad t w=u
$$
我们把 $x,y,w$ 消掉就得到了 $u = z+t(Ax-y)$，并且有
$$
0 \in-A \partial f^{*}\left(-A^{T} u\right)+\partial g^{*}(u)+\frac{1}{t}(u-z) 
$$
上面这个式子就等价于 $u=\text{prox}_{th}(z)$。因此就有 $\text{prox}_{th}(z) = z+t(A\hat{x}-\hat{y})$。

## 4. Moreau–Yosida smoothing

这一部分是从另一个角度看待 PPA 算法。我们知道如果 $f$ 是光滑函数就可以直接用梯度下降了，如果是非光滑函数则可以用次梯度或者近似点算法，前面复习 PG 方法的时候提到了 PG 也可以看成是一种梯度下降，梯度为 $G_t(x)$
$$
x_{k+1}=\text{prox}_{th}(x_k-t_k\nabla g(x_k)) \iff x^+ = x-tG_t(x)
$$
这一部分要讲的就是从梯度下降的角度认识 PPA 方法。

这里再次先抛出结论：**PPA 实际上就是对 $f$ 的某个光滑近似函数 $\tilde{f}$ 做梯度下降**。

这个光滑近似函数是什么呢？对于闭凸函数 $f$，我们定义
$$
\begin{aligned}
f_{(t)}(x) &=\inf _{u}\left(f(u)+\frac{1}{2 t}\|u-x\|_{2}^{2}\right) \quad(\text { with } t>0) \\
&=f\left(\operatorname{prox}_{t f}(x)\right)+\frac{1}{2 t}\left\|\operatorname{prox}_{t f}(x)-x\right\|_{2}^{2}
\end{aligned}
$$
为函数 $f$ 的 **Moreau Envelop** 。这里是将 $\text{prox}_{tf}(x)$ 代回到了原函数中。在此之前我们需要首先研究一下这个函数的性质。

1. $f_{(t)}$ 为**凸函数**。取 $G(x,u) = f(u)+\frac{1}{2t}\Vert u-x\Vert^2_2$ 是关于 $(x,u)$ 的联合凸函数，因此 $f_{(t)}(x)=\inf_u G(x,u)$ 是凸的；
2. $\text{dom}f_{(t)}=R^n$。这是因为 $\text{prox}_{tf}(x)$ 对任意的 $x$ 都有唯一的定义；
3. $f_{(t)}\in C^1$ **连续**；

另外可以验证共轭函数为
$$
\left(f_{(t)}\right)^{\star}(y)=f^{\star}(y)+\frac{t}{2}\|y\|_{2}^{2}
$$
因此还有性质

4. $\left(f_{(t)}\right)^{\star}(y)$ 为 **t-强凸函数**，等价的有 $f_{(t)}(x)$ 为 **1/t-smooth**；

既然这个 $f_{(t)}$ 为 $C^1$ 连续的，那么他的梯度是什么呢？
$$
f_{(t)}(x) = \left(f_{(t)}(x)\right)^{\star\star}=\sup _{y}\left(x^{T} y-f^{\star}(y)-\frac{t}{2}\|y\|_{2}^{2}\right)
$$
根据 Legendre transform 有 $y^\star\in\partial f_{(t)}(x)$，令上面式子关于 $y$ 的次梯度等于 0 可以得到
$$
x-ty^\star \in \partial f^\star(y^\star) \iff y^\star\in\partial f(x-ty^\star) \\\Longrightarrow x-ty^\star=\text{prox}_{tf}(x)
$$
因此我们就有(**重要**)
$$
y^\star=\nabla f_{(t)}(x) = \frac{1}{t}\left(x-\text{prox}_{tf}(x)\right)
$$
变换一下就是 $\text{prox}_{tf}(x) = x-t\nabla f_{(t)}(x)$，注意这个式子左边就是 PPA 的迭代方程，右边就是光滑函数函数 $f_{(t)}(x)$ 应用梯度下降法的迭代方程，并且由于这个函数是 $L=1/t$-smooth 的，因此我们取的步长为 $t$ 满足要求 $0<t\le 1/L$。也就是我们这一小节刚开始说的，PPA 等价于对一个光滑近似函数 $f_{(t)}(x)$ 的梯度下降方法。

***例子 1***：举个例子算一下 Moreau Envelop，假如函数 $f(x)=\delta_C(x)$，则 $f_{(t)}(x)=\frac{1}{2t}d(x)^2$，这里 $d(x)$ 是 $x$ 到集合 $C$ 的欧氏距离。

***例子 2***：若 $f(x)=\Vert x\Vert_1$，函数 $f_{(t)}(x)=\sum_k \phi_t(x_k)$ 被称为 **Huber penalty**，其中
$$
\phi_{t}(z)=\left\{\begin{array}{ll}z^{2} /(2 t) & |z| \leq t \\|z|-t / 2 & |z| \geq t\end{array}\right.
$$
![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/22-huber.PNG)

