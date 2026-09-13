---
title: 凸优化笔记25：原始对偶问题 PDHG
date: 2020-05-24 10:41:17
tags:
  - 近似点算子
  - 原始对偶问题
  - PPA
  - 算子分裂法
  - PDHG
categories:
  - Convex Optimization
mathjax: true
---

前面的章节要么从原始问题出发，要么从对偶问题出发，通过求解近似点或者一个子优化问题进行迭代，而且推导过程中我们发现根据问题的参数特征，比如矩阵 $A$ 是瘦高型的还是矮胖型的，采用对偶和原始问题的复杂度会不一样，可以选择一个更简单的。而这一节，我们将要从**原始对偶问题**出发来优化，什么是原始对偶问题呢？就是原始优化变量和对偶优化变量（原始函数和共轭函数）混合在一块，看下面的原理就知道了。

<!--more-->

## 1. 原始对偶问题

现在考虑原始优化问题，其中 $f,g$ 为闭凸函数
$$
\min \quad f(x)+g(Ax)
$$
这个问题我们前面遇到好多次了，一般都是取 $y=Ax$ 加一个约束条件然后计算拉格朗日函数（自己拿小本本写一下），再求解 KKT 条件对吧。好，让我们列出来 KKT 条件：

1. 原始可行性：$x\in\text{dom}f,Ax=y\in\text{dom}g$
2. $x,z$ 是拉格朗日函数的最小值点，因此 $-A^Tz\in\partial f(x),z\in\partial g(y)$

其中 $z\in\partial g(y)\iff Ax=y\in\partial g^\star(z)$。也就是说，要想求解 KKT 条件，我们需要的实际上是求解下面一个“方程”
$$
0 \in\left[\begin{array}{cc}0 & A^{T} \\-A & 0\end{array}\right]\left[\begin{array}{l}x \\z\end{array}\right]+\left[\begin{array}{c}\partial f(x) \\\partial g^{\star}(z)\end{array}\right]
$$

> **Remarks**：这个式子可重要啦，后面还会用到！而且他从集合的角度揭示了我们求解最优值问题的本质，那就是找一个**包含关系**。
>
> 比如上面的这个式子我们用一个算子来表示为 $T(x,z)$，我们求解最优值实际上要就是找满足 $0\in T(x,z)$ 的解 $(x^\star,z^\star)$。而对一个简单的优化问题 $\min f(x)$，我们实际上就是在找满足 $0\in\partial f(x)$ 的 $x^\star$，这个时候我们可以把次梯度看作是一个算子。
>
> 在这一章的后面几个小节，我们将从算子的角度重新来看待优化问题，看完之后可以再回到这里细细品味。

好我们先把这个东西放一放，再来看看另一个跟拉格朗日函数有关的函数
$$
\begin{aligned}
h(x,z)&=\inf_{y}L(x,y,z)\\
&=f(x)-g^\star(z)+z^TAx
\end{aligned}
$$
如果计算 $0\in\partial h(x,z)$ 是不是就是上面那个方程？！也就是说上面很重要的那个方程实际上就是在求解 $h(x,z)$ 的鞍点！很容易理解，因为 KKT 条件本质上就是在求拉格朗日函数的鞍点（当然，如果存在不等式约束就不一定是鞍点了）。大家注意，你看这个 $h$ ~~他又长又宽~~，这个 $h$ 同时包含了原始变量 $x$ 和对偶变量 $z$，同时还既有原始函数 $f$ 又有对偶函数 $g^\star$，所以我们叫他原始对偶优化问题，$h$ 也是部分拉格朗日函数(partial Lagrangian)。

## 2. PDHG

前面说了我们要求解的问题是
$$
0 \in\left[\begin{array}{cc}
0 & A^{T} \\
-A & 0
\end{array}\right]\left[\begin{array}{l}
x \\
z
\end{array}\right]+\left[\begin{array}{c}
\partial f(x) \\
\partial g^{\star}(z)
\end{array}\right]
$$

> **PDHG(Primal-dual hybrid gradient method)**的迭代格式是这样的
> $$
> \begin{array}{l}
> x_{k+1}=\operatorname{prox}_{\tau f}\left(x_{k}-\tau A^{T} z_{k}\right) \\
> z_{k+1}=\operatorname{prox}_{\sigma g^{*}}\left(z_{k}+\sigma A\left(2 x_{k+1}-x_{k}\right)\right)
> \end{array}
> $$
> 步长需要满足 $\sigma\tau\|A\|_2^2\le1$。

是不是看起来跟 DR 方法很像呢？事实上他们两个是等价的，后面会证明。回忆 ADMM，我们每次需要求解的优化问题是
$$
x^{k+1}=\arg\min_x f(x)+\frac{t}{2}\| Ax-y^k+\frac{z^k}{t}\|^2
$$
要求解这个优化问题，我们往往会得到一个线性方程，还需要计算 $(A^TA)^{-1}$，这就很麻烦了。但是观察 PDHG 的迭代格式，我们只需要求解 $f,g^\star$ 的 $\text{prox}$ 算子，我们只需要求解 $A,A^T$ 之间的乘法而不需要求逆了，这就方便很多了。

![example](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/25-example.png)

看上面这个例子，我们前面说过 ADMM 等价于 dual DR，不过这个例子里边 PDHG 是最慢的。

下面我们就来证明一下如何从 PDHG 导出 DR 方法。

对于优化问题 $\min f(x)+g(x)$，实际上相当于 $\min f(x)+g(Ax),A=I$，另外我们再取 PDHG 中的 $\sigma=\tau=1$，那么就可以得到
$$
\begin{array}{l}
x_{k+1}=\operatorname{prox}_{f}\left(x_{k}-z_{k}\right) \\
z_{k+1}=\operatorname{prox}_{g^{*}}\left(z_{k}+2 x_{k+1}-x_{k}\right)
\end{array}
$$
这实际上就是 DR Splitting 那一节讲的原始对偶形式。

另外也可以从 DR 方法导出 PDHG。我们可以将原问题 $\min f(x)+g(A x)$ 改写为
$$
\begin{aligned}
\operatorname{minimize} &\quad f(x)+g(A x+B y) \\
\text{subject to} &\quad y=0
\end{aligned}
$$
这里边我们可以选择 $B$ 使 $AA^T+BB^T=(1/\alpha)I$，其中 $1/\alpha\ge\|A\|_2^2$。为什么这么选呢？令 
$$
\tilde{g}(x,y)=g(Ax+By)=g\left(\tilde{A}\left(\begin{array}{c}x\\ y\end{array}\right)\right)
$$
那么就有 $\tilde{A}\tilde{A}^T=(1/\alpha)I$，而前面讲 $\text{prox}$ 算子的时候我们讲了一个性质，满足这个条件的时候 $\text{prox}_{\tilde{g}}$ 可以用 $\text{prox}_g$ 来表示。

> 复习：$\text{prox}$ 算子的性质
>
> $f(x)=g(Ax+b)$，对于一般的 $A$ 并不能得到比较好的性质，但如果 $AA^T=(1/\alpha)I$，则有 
> $$
> \begin{aligned}\operatorname{prox}_{f}(x) &=\left(I-\alpha A^{T} A\right) x+\alpha A^{T}\left(\operatorname{prox}_{\alpha^{-1} g}(A x+b)-b\right) \\&=x-\alpha A^{T}\left(A x+b-\operatorname{prox}_{\alpha^{-1} g}(A x+b)\right)\end{aligned}
> $$

我们还可以取 $\tilde{f}(x,y)=f(x)+\delta_{0}(y)$，那么优化问题就变成了 $\min \tilde{f}(x,y)+\tilde{g}(x,y)$，应用 DR 方法迭代格式为
$$
\begin{array}{l}
{\left[\begin{array}{c}
x_{k+1} \\
y_{k+1}
\end{array}\right]=\operatorname{prox}_{\tau \tilde{f}} \left(\left[\begin{array}{c}
x_{k}-p_{k} \\
y_{k}-q_{k}
\end{array}\right]\right)} \\
{\left[\begin{array}{c}
p_{k+1} \\
q_{k+1}
\end{array}\right]=\operatorname{prox}_{(\tau \tilde{g})^{*}}\left(\left[\begin{array}{c}
p_{k}+2 x_{k+1}-x_{k} \\
q_{k}+2 y_{k+1}-y_{k}
\end{array}\right]\right)}
\end{array}
$$
我们需要计算 $\text{prox}_{\tilde{f}}$ 和 $\text{prox}_{\tilde{g}}$
$$
\operatorname{prox}_{\tau \tilde{f}}(x, y)=\left[\begin{array}{c}
\operatorname{prox}_{\tau f}(x) \\
0
\end{array}\right]
$$
$$
\begin{aligned}
\operatorname{prox}_{\tau \tilde{g}}(x, y) &=\left[\begin{array}{c}
x \\
y
\end{array}\right]-\alpha\left[\begin{array}{c}
A^{T} \\
B^{T}
\end{array}\right]\left(A x+B y-\operatorname{prox}_{(\tau / \alpha) g}(A x+B y)\right.\\
&=\left[\begin{array}{c}
x \\
y
\end{array}\right]-\tau\left[\begin{array}{c}
A^{T} \\
B^{T}
\end{array}\right] \operatorname{prox}_{\sigma g^{\star}}(\sigma(A x+B y)) \\
&=\left[\begin{array}{c}
x \\
y
\end{array}\right]-\operatorname{prox}_{(\tau \tilde{g})^\star}(x, y)
\end{aligned}
$$

其中 $\sigma=\alpha/\tau$。代入到 DR 方法的迭代方程
$$
\begin{array}{l}
{\left[\begin{array}{c}
x_{k+1} \\
y_{k+1}
\end{array}\right]=\left[\begin{array}{c}
\operatorname{prox}_{\tau f}\left(x_{k}-p_{k}\right) \\
0
\end{array}\right]} \\
{\left[\begin{array}{c}
p_{k+1} \\
q_{k+1}
\end{array}\right]=\tau\left[\begin{array}{c}
A^{T} \\
B^{T}
\end{array}\right] \operatorname{prox}_{\sigma g^{*}}\left(\sigma\left[\begin{array}{cc}
A & B
\end{array}\right]\left[\begin{array}{c}
p_{k}+2 x_{k+1}-x_{k} \\
q_{k}+2 y_{k+1}-y_{k}
\end{array}\right]\right)}
\end{array}
$$
根据第二个式子应该有 $\left[\begin{array}{c} p_{k} \\ q_{k} \end{array}\right] \in \text { range }\left[\begin{array}{c} A^{T} \\ B^{T} \end{array}\right]$，因此存在 $z_k$ 满足 $\left[\begin{array}{c}p_{k+1} \\
q_{k+1}\end{array}\right]=\tau\left[\begin{array}{c}A^{T} \\ B^{T}\end{array}\right]z_k$。同时因为 $AA^T+BB^T=(1/\alpha)I$，所以能找到唯一的 $z_k$ 同时满足 $z_k=\sigma(Ap_k+Bq_k)$。那么把 $z_k$ 代入到上面的迭代方程，同时消掉 $y_k=0$，就可以得到
$$
x_{k+1}=\operatorname{prox}_{\tau f}\left(x_{k}-\tau A^{T} z_{k}\right)\\ z_{k+1}=\operatorname{prox}_{\sigma g^{*}}\left(z_{k}+\sigma A\left(2 x_{k+1}-x_{k}\right)\right)
$$
这就是 PDHG 算法，其中 $\sigma\tau=\alpha\le 1/\|A\|^2$。

当然，我们还可以对 PDHG 算法进行改进，比如：

**PDHG withover relaxation**：$\rho_k\in(0,2)$
$$
\begin{aligned}
\bar{x}_{k+1} &=\operatorname{prox}_{\tau f}\left(x_{k}-\tau A^{T} z_{k}\right) \\
\bar{z}_{k+1} &=\operatorname{prox}_{\sigma g^{*}}\left(z_{k}+\sigma A\left(2 \bar{x}_{k+1}-x_{k}\right)\right) \\
\left[\begin{array}{c}
x_{k+1} \\
z_{k+1}
\end{array}\right] &=\left[\begin{array}{c}
x_{k} \\
z_{k}
\end{array}\right]+\rho_{k}\left[\begin{array}{c}
\bar{x}_{k+1}-x_{k} \\
\bar{z}_{k+1}-z_{k}
\end{array}\right]
\end{aligned}
$$
其收敛性与 DR 方法相同。

**PDHG with acceleration**：
$$
\begin{array}{l}
x_{k+1}=\operatorname{prox}_{\tau_{k} f}\left(x_{k}-\tau_{k} A^{T} z_{k}\right) \\
z_{k+1}=\operatorname{prox}_{\sigma_{k} g^{*}}\left(z_{k}+\sigma_{k} A\left(\left(1+\theta_{k}\right) x_{k+1}-\theta_{k} x_{k}\right)\right)
\end{array}
$$
对于强凸函数 $f$，以及适当的选择 $\sigma_k,\tau_k,\theta_k$，收敛速度可以达到 $1/k^2$。

## 3. 单调算子

单调算子(monotone operator)我们在讲次梯度的时候提到过，这次我们从算子的角度理解一下 PDHG 方法。

### 3.1 集值算子

集值算子(Multivalued/set-valued operator)，就是说映射得到的不是单个的值，而是一个集合。比如算子 $F$ 把向量 $x\in R^n$ 映射到集合 $F(x)\subseteq R^n$。有两个定义

- 定义域 $\operatorname{dom} F =\left\{x \in \mathbf{R}^{n} | F(x) \neq \emptyset\right\}$
- 图 $\operatorname{gr}(F) =\left\{(x, y) \in \mathbf{R}^{n} \times \mathbf{R}^{n} | x \in \operatorname{dom} F, y \in F(x)\right\}$

![set-valued](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/25-set-valued.png)

对算子放缩、求逆等操作都可以表示为对“**图**”的**线性变换**。

**求逆**：$F^{-1}(x)=\{y| x\in F(y)\}$
$$
\operatorname{gr}\left(F^{-1}\right)=\left[\begin{array}{cc}
0 & I \\
I & 0
\end{array}\right] \operatorname{gr}(F)
$$
**放缩**：$(\lambda F)(x)=\lambda F(x)$ and $(F\lambda)(x)=F(\lambda x)$
$$
\operatorname{gr}(\lambda F)=\left[\begin{array}{cc}
I & 0 \\
0 & \lambda I
\end{array}\right] \operatorname{gr}(F), \quad \operatorname{gr}(F \lambda)=\left[\begin{array}{cc}
(1 / \lambda) I & 0 \\
0 & I
\end{array}\right] \operatorname{gr}(F)
$$
**相加**：$(I+\lambda F)(x)=\{x+\lambda y | y \in F(x)\}$
$$
\operatorname{gr}(I+\lambda F)=\left[\begin{array}{cc}
I & 0 \\
I & \lambda I
\end{array}\right] \operatorname{gr}(F)
$$
注意 $(I+\lambda F)$ 这个形式很特别，如果我们取 $F(x)=\partial f(x)$，那么 $(I+\lambda F)^{-1}$ 实际上就是 $\text{prox}$ 算子（$\lambda>0$），不过我们给他取了另一个名字 **Resolvent**，$y\in(I+\lambda F)^{-1}(x)\iff x-y\in\partial f(y)$，用图来表示就是
$$
\operatorname{gr}\left((I+\lambda F)^{-1}\right)=\left[\begin{array}{cc}
I & \lambda I \\
I & 0
\end{array}\right] \operatorname{gr}(F)
$$
***例子 1***：$(I+\lambda \partial f(x))^{-1}=\text{prox}_{\lambda f}(x)$

***例子 2***：$F(x)=Ax+b$，$(I+\lambda F)^{-1}(x)=(I+\lambda A)^{-1}(x-\lambda b)$，后面这个求逆完全就是矩阵求逆。

![transformation](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/25-transformation.PNG)

### 3.2 单调算子

**定义**：$F$ 是单调算子，若
$$
(y-\hat{y})^{T}(x-\hat{x}) \geq 0 \quad \text { for all } x, \hat{x} \in \operatorname{dom} F, y \in F(x), \hat{y} \in F(\hat{x})
$$
如果用图表示，就应该有
$$
\left[\begin{array}{c}x-\hat{x} \\y-\hat{y}\end{array}\right]^{T}\left[\begin{array}{cc}0 & I \\I & 0\end{array}\right]\left[\begin{array}{c}x-\hat{x} \\y-\hat{y}\end{array}\right] \geq 0 \quad \text { for all }(x, y),(\hat{x}, \hat{y}) \in \operatorname{gr}(F) \quad (\bigstar)
$$
上面这个式子很重要！！！后面会多次用到。

***例子***：我们需要用到的单调算子有：

1. 凸函数次梯度 $\partial f(x)$
2. 仿射变换 $F(x)=Cx+d$，并且需要满足 $C+C^T\succeq 0$
3. 他们的组合，比如

$$
F(x, z)=\left[\begin{array}{cc}
0 & A^{T} \\
-A & 0
\end{array}\right]\left[\begin{array}{c}
x \\
z
\end{array}\right]+\left[\begin{array}{c}
\partial f(x) \\
\partial g^{*}(z)
\end{array}\right]
$$

除了单调算子，还有个**最大单调算子(Maximal monotone operator)**，也就是说它的图不能是其他任意单调算子的真子集，举个栗子就明白了，参考下面的图。我们可以知道b闭凸函数的偏导数、单调仿射变换是最大单调算子，除此之外，还有定理。

**Minty’s Theorem**：单调算子 $F$ 是最大单调算子当且仅当
$$
\operatorname{im}(I+F)=\bigcup_{x \in \operatorname{dom} F}(x+F(x))=\mathbf{R}^{n}
$$
![maximal-monotone](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/25-maximal-monotone.PNG)

除了单调性质，我们在证明收敛新的时候往往还要用到 Lipschitz 连续、强凸性质等等，实际上我们前面已经介绍过很多次了，而且用了一堆名词 coercivity、expansive、firmly nonexpansive，我实在是晕了......这里我们就再总结一下。假设算子 $F$ 有 $y=F(x),\hat{y}=F(\hat{x})$

| $(y-\hat{y})^T(x-\hat{x})\ge \mu \Vert x-\hat{x}\Vert^2,\mu>0$ | $(y-\hat{y})^T(x-\hat{x})\ge \gamma \Vert y-\hat{y}\Vert^2,\gamma>0$ | $(y-\hat{y})^T(x-\hat{x})\le L\Vert x-\hat{x}\Vert^2 $ |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------ |
| coercive                                                     | co-coercive                                                  |                                                        |
|                                                              | ﬁrmly nonexpansive($\gamma=1$)                               | nonexpansive($L\le 1$)                                 |
|                                                              |                                                              | Lipschitz continuous                                   |

**它们之间的关系**

1. 如果满足 co-coercive 并且有 $\gamma=1$，则其为 firmly nonexpansive
2. 如果满足 Lipschitz continuous 并且有 $L\le 1$，则其为 nonexpansive
3. co-coercivity 可以导出 Lipschitz continuous($L=1/\gamma$)，但反之不一定。不过对于闭凸函数他们是等价的。

**它们各自的性质**

**Coercivity**等价于 $F-\mu I$ 是一个单调算子，也等价于
$$
\left[\begin{array}{c}
x-\hat{x} \\
y-\hat{y}
\end{array}\right]^{T}\left[\begin{array}{cc}
-2 \mu I & I \\
I & 0
\end{array}\right]\left[\begin{array}{c}
x-\hat{x} \\
y-\hat{y}
\end{array}\right] \geq 0 \quad \text { for all }(x, y),(\hat{x}, \hat{y}) \in \operatorname{gr}(F)
$$
**Co-coercivity**等价于
$$
\left[\begin{array}{c}
x-\hat{x} \\
y-\hat{y}
\end{array}\right]^{T}\left[\begin{array}{cc}
0 & I \\
I & -2 \gamma I
\end{array}\right]\left[\begin{array}{c}
x-\hat{x} \\
y-\hat{y}
\end{array}\right] \geq 0 \quad \text { for all }(x, y),(\hat{x}, \hat{y}) \in \operatorname{gr}(F)
$$


对前面提到的 resolvent 来说，算子单调性有以下重要性质：

> 重要性质：**算子是单调的，当且仅当他的 resolvant 是 firmly nonexpansive**
>
> 证明只需要根据矩阵等式 $\lambda\left[\begin{array}{ll}0 & I \\ I & 0\end{array}\right]=\left[\begin{array}{cc}I & I \\ \lambda I & 0 \end{array}\right]\left[\begin{array}{cc}0 & I \\ I & -2 I \end{array}\right]\left[\begin{array}{cc}I & \lambda I \\ I & 0 \end{array}\right]$ 就可以得到（结合 $(\bigstar)$ 式）。

另外单调算子 $F$ 是最大单调算子，当且仅当
$$
\text{dom}(I+\lambda F)^{-1}=R^n
$$
这可以由 Minty’s theorem 得到。

## 4. 近似点算法

### 4.1 回望 PPA

前面讲到了 Resolvant  $(I+\lambda F)^{-1}$ 实际上就是近似点算子，而 PPA 就是在计算近似点，回忆 PPA 的迭代格式为
$$
\begin{aligned}
x_{k+1} &= \text{prox}_{t_k f}(x_k) \\
&= (1+t_k F)^{-1}(x_k)
\end{aligned}
$$
我们实际上就是在找 Resolvant 算子 $R_t=(I+t F)^{-1}$ 的**不动点**
$$
x=R_t(x)\iff x\in(1+tF)(x)\iff 0\in F(x)
$$
加入松弛后的 PPA 可以写成下面的形式，其中 $\rho_k\in(0,2)$ 为松弛参数
$$
x_{k+1}=x_k+\rho_k(R_{t_k}(x_k)-x_k)
$$
那么**收敛性**是怎么样呢？假如 $F^{-1}(0)\neq \varnothing$，在满足以下条件时 PPA 可以收敛

- $t_k=t>0,\rho_k=\rho\in(0,2)$ 都选择常数值；或者
- $t_k,\rho_k$ 随迭代次数变化，但是需要满足 $t_k\ge t_{\min}> 0,0<\rho_{\min}\le \rho_k\le \rho_{\max}< 2,\forall k$

这个收敛性的证明可以通过证明 Resolvant 的 firmly nonexpansiveness 性质来完成（可以去复习 DR 方法的收敛性证明，那里实际上也是一个不动点迭代）。

### 4.2 再看 PPA

先打个预防针，这一部分很重要！！！看完以后也许会对 PPA 以及其他优化算法有更多的理解！！！

首先我们回忆 PPA 是什么。对于优化问题 $\min f(x)$，迭代格式为 $x^+=\text{prox}_{tf}(x)$。如果我们把 $\partial f(x)$ 用算子 $F$ 来表示，那么优化问题实际上就是在找满足 $0\in F(x)$ 的解，PPA 实际上就是在找不动点 $x=(1+tF)^{-1}(x)$。

假如我们现在引入一个**非奇异矩阵** $A$，令 $x=Ay$ 代入到原方程（为什么要这么做？如果合适地选择 $A$ 的话，有时候可以使问题简化，跟着推导的思路看到最后就能理解了，来吧！）

记 $g(y)=f(Ay)$，那么优化问题变为了 $\min g(y)$，注意由于 $A$ 是非奇异的，所以这个问题跟原问题 $\min f(x)$ 是等价的。我们需要找满足 $0\in\partial g(y)=A^T\partial f(Ay)$ 的解，于是可以定义算子 $G(y)=\partial g(y)=A^TF(Ay)$，这个时候 $G$ 的图就是做一个线性变换
$$
\operatorname{gr}(G)=\left[\begin{array}{cc}A^{-1} & 0 \\0 & A^{T}\end{array}\right] \operatorname{gr}(F)
$$
如果 $F$ 是一个单调算子的话，那么 $G$ 也是一个**单调算子**，这是因为（结合 $(\bigstar)$ 式）
$$
\left[\begin{array}{cc}A^{-1} & 0 \\0 & A^{T}\end{array}\right]^{T}\left[\begin{array}{cc}0 & I \\I & 0\end{array}\right]\left[\begin{array}{cc}A^{-1} & 0 \\0 & A^{T}\end{array}\right]=\left[\begin{array}{cc}0 & I \\I & 0\end{array}\right]
$$
然后对 $\min g(y)$ 应用 PPA 迭代格式为
$$
y_{k+1}=(I+t_kG)^{-1}(y_k)
$$
我们把 $x_k=Ay_k,G=A^TF(Ay)$ 都代入进去，就能把上面的式子等价表示为
$$
\frac{1}{t_k}H(x_k-x)\in F(x)
$$
其中 $H=(AA^T)^{-1}\succ 0$。这个式子又可以表示为
$$
x_{k+1}=(H+t_kF)^{-1}(Hx_k)
$$
因为 $\min g(y)\iff \min f(x)$，所以上面这个迭代格式也完全适用于原问题，如果取 $H=I$ 那就是原始形式的 PPA，如果取别的形式，那么就获得了推广形式的 PPA！

引入 $A$ 有什么作用呢？我们看
$$
\frac{1}{t_k}H(x_k-x)\in F(x) \iff x_{k+1}=\arg\min_x\left(f(x)+\frac{1}{2t_k}\|x-x_k\|_H^2 \right)
$$
其中 $\|x\|_H^2=x^THx$，如果说 $f(x)=(1/2)\|Bx-b\|^2$，那么我们就可以选择 $A$ 使 $H=(1/\alpha) I-B^TB$，这样迭代求解 $x_{k+1}$ 就简单了。当然这个作用范围很有限，下面的例子更能显现他的威力。

我们再回到原始对偶问题，记算子 $F$ 为
$$
F(x,z)=\left[\begin{array}{cc}0 & A^{T} \\-A & 0\end{array}\right]\left[\begin{array}{l}x \\z\end{array}\right]+\left[\begin{array}{c}\partial f(x) \\\partial g^{\star}(z)\end{array}\right]
$$
优化问题就是要找到 $0\in F(x,z)$。如果用原始的 PPA 算法，迭代方程为 $(x_{k+1},z_{k+1})=(I+tF)^{-1}(x_k,z_k)$，$(x_{k+1},z_{k+1})$ 是下面方程的解

![ppa](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/25-ppa.PNG)

注意到 $x,z$ 纠缠在一起了，我们想把他们拆开来分别求解 $x,z$，问题就能更简单。怎么做呢，引入一个 $H$
$$
H=\left[\begin{array}{cc}I & -\tau A^{T} \\-\tau A & (\tau / \sigma) I\end{array}\right]
$$
其中若 $\sigma\tau\|A\|_2^2< 1$ 则 $H$ 为正定矩阵。这个时候 $(x_{k+1},z_{k+1})$ 就是下面方程的解
$$
\begin{array}{c}
{\frac{1}{\tau}\left[\begin{array}{cc}
I & -\tau A^{T} \\
-\tau A & (\tau / \sigma) I
\end{array}\right]\left[\begin{array}{c}
x_{k}-x \\
z_{k}-z
\end{array}\right] \in\left[\begin{array}{cc}
0 & A^{T} \\
-A & 0
\end{array}\right]\left[\begin{array}{c}
x \\
z
\end{array}\right]+\left[\begin{array}{c}
\partial f(x) \\
\partial g^{*}(z)
\end{array}\right] }\\
\Updownarrow \\
{\begin{array}{l}
0 \in \partial f(x)+\frac{1}{\tau}\left(x-x_{k}+\tau A^{T} z_{k}\right) \\
0 \in \partial g^{*}(z)+\frac{1}{\sigma}\left(z-z_{k}-\sigma A\left(2 x-x_{k}\right)\right)
\end{array} }\\
\Updownarrow \\
{\begin{aligned}
x_{k+1} &=(I+\tau \partial f)^{-1}\left(x_{k}-\tau A^{T} z_{k}\right) \\
z_{k+1} &=\left(I+\sigma \partial g^{*}\right)^{-1}\left(z_{k}+\sigma A\left(2 x_{k+1}-x_{k}\right)\right)
\end{aligned} }
\end{array}
$$
对于化简后的式子，我们就可以先单独求解 $x_{k+1}$，然后再求解 $z_{k+1}$。这实际上也是 PDHG 的迭代方程
$$
\begin{array}{l}x_{k+1}=\operatorname{prox}_{\tau f}\left(x_{k}-\tau A^{T} z_{k}\right) \\z_{k+1}=\operatorname{prox}_{\sigma g^{*}}\left(z_{k}+\sigma A\left(2 x_{k+1}-x_{k}\right)\right)\end{array}
$$
