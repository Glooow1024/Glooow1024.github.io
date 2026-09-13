---
title: 凸优化笔记23：算子分裂法 & ADMM
date: 2020-05-10 09:52:41
tags:
  - 近似点算子
  - 算子分裂法
  - ADMM
categories:
  - Convex Optimization
mathjax: true
---

前面章节中，针对 $\min f(x)+g(Ax)$ 形式的优化问题，我们介绍了如 PG、dual PG、ALM、PPA 等方法。但是比如 PG 方法为 
$$
x_{k+1}=\text{prox}_{th}(x_k-t_k\nabla g(x_k))
$$
ALM 的第一步要解一个联合优化问题 
$$
(x^{k+1},y^{k+1}) = \arg\min_{x,y} L_t(x,y,z^k)
$$
他们都把 $f,g$ 耦合在一起了。如果我们看原始问题 $\min f(x)+g(Ax)$ 实际上就是要找 $x^\star$ 使得 $0\in\partial f(x^\star)+A^T\partial g(x^\star)$，这一节要介绍的 Douglas-Rachford splitting method 实际上就是要 decoupling。

<!--more-->

## 1.Douglas-Rachford splitting Algorithm

针对如下优化问题，其中 $f,g$ 都是闭凸函数
$$
\min f(x)+g(x)
$$

> 先给出 DR-splitting 方法的迭代方程
> $$
> \begin{array}{l}
> x_{k+1}=\operatorname{prox}_{f}\left(y_{k}\right) \\
> y_{k+1}=y_{k}+\operatorname{prox}_{g}\left(2 x_{k+1}-y_{k}\right)-x_{k+1}
> \end{array}
> $$

为什么叫做 splitting 呢？回忆 PPA 是不是需要求解 $x^+ = \text{prox}_{t(f+g)}(x)$，而这里则可以分开依次求 $\text{prox}_f$ 和 $\text{prox}_g$，所以被称为 splitting。这个迭代方程看起来没有规律，那么他能不能收敛呢？答案当然是可以的，$x_k$ 最终会收敛到 $0\in \partial f(x)+\partial g(x)$，这个证明放到后面，先来从别的方面认识一下这个方法。

首先 $f,g$ 并没有区分，因此可以交换两者的位置，那么迭代方程也可以写为
$$
\begin{array}{l}
x_{k+1}=\operatorname{prox}_{g}\left(y_{k}\right) \\
y_{k+1}=y_{k}+\operatorname{prox}_{f}\left(2 x_{k+1}-y_{k}\right)-x_{k+1}
\end{array}
$$
但需要注意的是这两种不同的迭代方程产生的序列是不一样的，也可能会影响收敛的速度，因此这个方法关于 $f,g$ 是不对称的。

如果把 $x_{k+1}$ 带入到第二步，整个过程实际上可以用一个迭代方程表示
$$
y_{k+1} = F(y) \notag\\
F(y)=y+\operatorname{prox}_{g}\left(2 \operatorname{prox}_{f}(y)-y\right)-\operatorname{prox}_{f}(y)
$$
这是个什么式子呢？**不动点迭代**(fixed-point iteration)！就是在找函数 $F(y)$ 的不动点。这个函数 $F(y)$ 是连续的吗？是的，这是因为上一节中我们证明了 $\text{prox}_{h}(x)$ 满足firmly nonexpansive(co-coercivite) 性质
$$
\left(\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right)^{T}(x-y) \geq\left\|\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right\|_{2}^{2}
$$
因此近似点算子是 Lipschitz continuous 的，所以 $F(y)$ 也是连续的。那么假如最终找到了不动点 $y$，他有什么性质呢？
$$
y=F(y) \notag\\
\iff 0 \in \partial f\left(\operatorname{prox}_{f}(y)\right)+\partial g\left(\operatorname{prox}_{f}(y)\right)
$$
**证明**：对于不动点 $y=F(y)$，取 $x=\text{prox}_f(y)$，我们有
$$
\begin{aligned}
x=\text{prox}_f(y),&\quad F(y)=y \notag\\
\iff x=\text{prox}_f(y),&\quad x=\text{prox}_g(2x-y) \\
\iff y-x\in \partial f(x),&\quad x-y\in\partial g(x)
\end{aligned}
$$
其中第一个等价性只需要把 $x$ 带入到 $F(y)$ 中，由此我们就可以得到
$$
0=(y-x)+(x-y)\in\partial f(x)+\partial g(x)
$$
自然而然地我们证明了一开始提到的 $x_{k}$ 的收敛性。

**等价形式**：下面这部分则主要是对原始形式做了一些**变量代换**，使其看起来更简洁，并没有新的内容。首先交换 $x,y$ 的迭代次序
$$
\begin{array}{l}
y_{k+1}=y_{k}+\operatorname{prox}_{g}\left(2 x_{k}-y_{k}\right)-x_{k} \\
x_{k+1}=\operatorname{prox}_{f}\left(y_{k+1}\right)
\end{array}
$$
引入新变量 $u_{k+1}=\text{prox}_g(2x_k-y_k),w_k=x_k-y_k$
$$
\begin{aligned}
u_{k+1} &=\operatorname{prox}_{g}\left(x_{k}+w_{k}\right) \\
x_{k+1} &=\operatorname{prox}_{f}\left(u_{k+1}-w_{k}\right) \\
w_{k+1} &=w_{k}+x_{k+1}-u_{k+1}
\end{aligned}
$$
**放缩**：除此之外，我们还可以对原始问题做一个放缩变为 $\min tf(x)+tg(x)$，那么迭代方程就变为如下形式，并没有本质的变化
$$
\begin{aligned}
u_{k+1} &=\operatorname{prox}_{tg}\left(x_{k}+w_{k}\right) \\
x_{k+1} &=\operatorname{prox}_{tf}\left(u_{k+1}-w_{k}\right) \\
w_{k+1} &=w_{k}+x_{k+1}-u_{k+1}
\end{aligned}
$$
**松弛**：前面降到了实际上是在对 $y$ 做不动点迭代，那么我们可以改为
$$
y_{k+1}=y_{k}+\rho_{k}\left(F\left(y_{k}\right)-y_{k}\right)
$$
如果 $1<\rho_k<2$ 就是超松弛，如果 $0<\rho_k<1$ 就是低松弛。这个时候迭代方程稍微复杂了一点点
$$
\begin{aligned}
u_{k+1} &=\operatorname{prox}_{g}\left(x_{k}+w_{k}\right) \\
x_{k+1} &=\operatorname{prox}_{f}\left(x_{k}+\rho_{k}\left(u_{k+1}-x_{k}\right)-w_{k}\right) \\
w_{k+1} &=w_{k}+x_{k+1}-x_{k}+\rho_{k}\left(x_{k}-u_{k+1}\right)
\end{aligned}
$$
**共轭函数**：根据 Moreau decomposition $\text{prox}_g(x)+\text{prox}_{g^\star}(x)=x$，如果 $\text{prox}_g$ 比较难计算，我们就可以换到共轭函数上去计算
$$
\begin{array}{l}
x_{k+1}=\operatorname{prox}_{f}\left(y_{k}\right) \\
y_{k+1}=x_{k+1}-\operatorname{prox}_{g^{*}}\left(2 x_{k+1}-y_{k}\right)
\end{array}
$$
下面举几个例子，主要就是练习近似点算子的计算，因为 DR-splitting 方法主要就是在计算 $f,g$ 的近似点。

***例子 1***：变量 $X\in S^n$，参数 $C\in S_+^n,\gamma>0$
$$
\text { minimize } \quad \operatorname{tr}(C X)-\log \operatorname{det} X+\gamma \sum_{i>j}\left|X_{i j}\right|
$$
我们取 $f(X)=\operatorname{tr}(C X)-\log \operatorname{det} X,\quad g(X)=\gamma \sum_{i>j}\left|X_{i j}\right|$

$X=\text{prox}_{tf}(\hat{X}) \iff C-X^{-1}+(1/t)(X-\hat{X})$，这个方程可以通过对 $\hat{X}-tC$ 进行特征值分解求解

$X=\text{prox}_{tg}(\hat{X})$ 可以通过软阈值(soft-thresholding)求解

***例子 2***：考虑等式约束的优化问题
$$
\begin{aligned}
\min \quad& f(x)\\
\text{s.t.} \quad& x\in V
\end{aligned}
$$
等价于 $g=\delta_V$
$$
\begin{array}{l}
x_{k+1}=\operatorname{prox}_{g}\left(y_{k}\right) \\
y_{k+1}=y_{k}+P_V\left(2 x_{k+1}-y_{k}\right)-x_{k+1}
\end{array}
$$
***例子 3***：考虑这种复合形式 $\min f_1(x)+f_2(Ax)$，可以引入等式约束
$$
\begin{aligned}
\min \quad& f_1(x)+f_2(y) \\
\text{s.t.} \quad& Ax=y
\end{aligned}
$$
取 $f(x_1,x_2)=f_1(x_1)+f_2(x_2)$，他的近似点算子是可分的
$$
\operatorname{prox}_{t f}\left(x_{1}, x_{2}\right)=\left(\operatorname{prox}_{t f_{1}}\left(x_{1}\right), \operatorname{prox}_{t f_{2}}\left(x_{2}\right)\right)
$$
然后像例子 2 一样，向超平面 $[A,-I][x_1,x_2]^T=0$ 做个投影。

## 2. ADMM

交替方向乘子法(Alternating Direction Method of Multipliers)也是一个很重要而且很受欢迎的算法，下一节还会详细讲，这里主要是看看他与 DR-splitting 的联系。

这里还是先给出结论：**DR-splitting 中取 $\rho_k=1$，应用在对偶问题上，就等价于原问题的 ADMM 算法**。我们先推导对偶问题上的 DR-splitting 迭代形式，然后再引出 ADMM 方法。

对可分离的凸优化问题
$$
\begin{aligned}
(P)\min \quad& f_1(x_1)+f_2(x_2) \\
\text{s.t.} \quad& A_1x_1+A_2x_2=b \\
(D)\max \quad& -b^{T} z-f_{1}^{*}\left(-A_{1}^{T} z\right)-f_{2}^{*}\left(-A_{2}^{T} z\right)
\end{aligned}
$$
取 $g(z)=b^{T} z+f_{1}^{\star}\left(-A_{1}^{T} z\right), f(z)=f_{2}^{\star}\left(-A_{2}^{T} z\right)$，DR 方法为
$$
u^{+}=\operatorname{prox}_{t g}(z+w), \quad z^{+}=\operatorname{prox}_{t f}\left(u^{+}-w\right), \quad w^{+}=w+z^{+}-u^{+}
$$
**第一步**：他等价于计算
$$
\begin{aligned}
\hat{x}_{1} &=\underset{x_{1}}{\operatorname{argmin}}\left(f_{1}\left(x_{1}\right)+z^{T}\left(A_{1} x_{1}-b\right)+\frac{t}{2}\left\|A_{1} x_{1}-b+w / t\right\|_{2}^{2}\right) \\
u^{+} &=z+w+t\left(A_{1} \hat{x}_{1}-b\right)
\end{aligned}
$$
这个证明很不直观，上一节分析 PPA 与 ALM 的关系的时候，证明了一个很不直观的结论：对 $h(z)=g^{\star}(z)+f^{\star}\left(-A^{T} z\right)$，有
$$
\begin{aligned}
z^+&=\text{prox}_{th}(z) = z+t(A\hat{x}-\hat{y}) \\
(\hat{x}, \hat{y})&=\underset{x, y}{\operatorname{argmin}}\left(f(x)+g(y)+z^{T}(A x-y)+\frac{t}{2}\|A x-y\|_{2}^{2}\right)
\end{aligned}
$$
**第二步**：与第一个式子是类似的，等价于
$$
\begin{array}{l}
\hat{x}_{2}=\underset{x_{2}}{\operatorname{argmin}}\left(f_{2}\left(x_{2}\right)+z^{T} A_{2} x_{2}+\frac{t}{2}\left\|A_{1} \hat{x}_{1}+A_{2} x_{2}-b\right\|_{2}^{2}\right. \\
z^{+}=z+t\left(A_{1} \hat{x}_{1}+A_{2} \hat{x}_{2}-b\right)
\end{array}
$$
**第三步**：$w^+=tA_2\hat{x}_2$

现在我们就可以引出 ADMM 方法了，他包括三个步骤
$$
\begin{aligned}
x_{k+1,1}&=\underset{\tilde{x}_{1}}{\operatorname{argmin}}\left(f_{1}\left(\tilde{x}_{1}\right)+z_{k}^{T} A_{1} \tilde{x}_{1}+\frac{t}{2}\left\|A_{1} \tilde{x}_{1}+A_{2} x_{k, 2}-b\right\|_{2}^{2}\right) \\
x_{k+1,2}&=\underset{\tilde{x}_{2}}{\operatorname{argmin}}\left(f_{2}\left(\tilde{x}_{2}\right)+z_{k}^{T} A_{2} \tilde{x}_{2}+\frac{t}{2}\left\|A_{1} x_{k+1,1}+A_{2} \tilde{x}_{2}-b\right\|_{2}^{2}\right) \\
z_{k+1}&=z_{k}+t\left(A_{1} x_{k+1,1}+A_{2} x_{k+1,2}-b\right)
\end{aligned}
$$
前两步分别对应了增广拉格朗日函数的两部分，分别对 $x_1,x_2$ 进行优化。与原本的 ALM 算法相比，ALM 是每次对 $(x_1,x_2)$ 进行联合优化，即 
$$
\begin{aligned}
(x_{k+1,1},x_{k+1,2}) = \arg\min_{x_1,x_2} L_t(x_1,x_2,z_k) \\
z_{k+1} = z_k + t\left(A_{1} x_{k+1,1}+A_{2} x_{k+1,2}-b\right)
\end{aligned}
$$
另外我们前面还讲到了 dual PG 方法跟 ALM 也很像，也是增广拉格朗日函数先对 $x_1$ 优化再对 $x_2$ 优化，但注意他跟 ADMM 不同的地方在于：前者对 $x_1$ 优化的时候不包含后面的二次正则项，而 ADMM 则包含，写出来对比一下就知道了
$$
\begin{aligned}
(dual\ PG)\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+z^{T} A x\right) \\
\hat{y} &=\underset{y}{\operatorname{argmin}}\left(g(y)-z^{T} y+\frac{t}{2}\|A \hat{x}-y\|_{2}^{2}\right) \\
(ADMM) x_{k+1,1}&=\underset{\tilde{x}_{1}}{\operatorname{argmin}}\left(f_{1}\left(\tilde{x}_{1}\right)+z_{k}^{T} A_{1} \tilde{x}_{1}+\frac{t}{2}\left\|A_{1} \tilde{x}_{1}+A_{2} x_{k, 2}-b\right\|_{2}^{2}\right) \\
x_{k+1,2}&=\underset{\tilde{x}_{2}}{\operatorname{argmin}}\left(f_{2}\left(\tilde{x}_{2}\right)+z_{k}^{T} A_{2} \tilde{x}_{2}+\frac{t}{2}\left\|A_{1} x_{k+1,1}+A_{2} \tilde{x}_{2}-b\right\|_{2}^{2}\right)
\end{aligned}
$$

## 3. 收敛性分析

DR 方法可以看成是一个不动点迭代，因此要证明收敛性，我们需要证明以下两个结论：

1. $y_k$ 收敛到 $F(y)$ 的不动点 $y^\star$
2. $x_{k+1}=\text{prox}_f(y_k)$ 收敛到 $x^\star=\text{prox}_f(y^\star)$

在证明收敛性之前，需要先定义两个函数
$$
\begin{aligned}F(y) &=y+\operatorname{prox}_{g}\left(2 \operatorname{prox}_{f}(y)-y\right)-\operatorname{prox}_{f}(y) \\G(y) &=y-F(y) \\&=\operatorname{prox}_{f}(y)-\operatorname{prox}_{g}\left(2 \operatorname{prox}_{f}(y)-y\right)\end{aligned}
$$
需要用到的是这两个函数的 firmly nonexpansive(co-coercive with parameter 1) 的性质
$$
\begin{aligned}(F(y)-F(\hat{y}))^{T}(y-\hat{y}) &\geq\|F(y)-F(\hat{y})\|_{2}^{2} \quad \text { for all } y, \hat{y} \\(G(y)-G(\hat{y}))^{T}(y-\hat{y}) &\geq\|G(y)-G(\hat{y})\|_{2}^{2}\end{aligned}
$$
**证明**：令 $x=\text{prox}_f(y),\hat{x}=\text{prox}_f(\hat{y})$，$v=\operatorname{prox}_{g}(2 x-y), \quad \hat{v}=\operatorname{prox}_{g}(2 \hat{x}-\hat{y})$

根据 $F(y)=y+v-x,F(\hat{y})=\hat{y}+\hat{v}-\hat{x}$ 有
$$
\begin{array}{l}(F(y)-F(\hat{y}))^{T}(y-\hat{y}) \\\quad \geq \quad(y+v-x-\hat{y}-\hat{v}+\hat{x})^{T}(y-\hat{y})-(x-\hat{x})^{T}(y-\hat{y})+\|x-\hat{x}\|_{2}^{2} \\\quad=(v-\hat{v})^{T}(y-\hat{y})+\|y-x-\hat{y}+\hat{x}\|_{2}^{2} \\\quad=(v-\hat{v})^{T}(2 x-y-2 \hat{x}+\hat{y})-\|v-\hat{v}\|_{2}^{2}+\|F(y)-F(\hat{y})\|_{2}^{2} \\\quad \geq\|F(y)-F(\hat{y})\|_{2}^{2}\end{array}
$$
其中用到了 $\text{prox}$ 算子的firm nonexpansiveness 性质
$$
(x-\hat{x})^{T}(y-\hat{y}) \geq\|x-\hat{x}\|_{2}^{2}, \quad(2 x-y-2 \hat{x}+\hat{y})^{T}(v-\hat{v}) \geq\|v-\hat{v}\|_{2}^{2}
$$
证毕。

然后我们就可以根据以下的不动点迭代方程证明前面提到的收敛性
$$
\begin{aligned}y_{k+1} &=\left(1-\rho_{k}\right) y_{k}+\rho_{k} F\left(y_{k}\right) \\&=y_{k}-\rho_{k} G\left(y_{k}\right)\end{aligned}
$$
其中需要假设 $F$ 的不动点存在，且满足 $0\in\partial f(x)+\partial g(x)$，以及松弛变量 $\rho_k\in [\rho_{\min},\rho_{\max}],0<\rho_{\min}<\rho_{\max}<2$。

**证明**：设 $y^\star$ 为 $F(y)$ 的不动点（也即 $G(y)$ 的零点），考虑第 $k$ 步迭代
$$
\begin{aligned}\left\|y^{+}-y^{\star}\right\|_{2}^{2}-\left\|y-y^{\star}\right\|_{2}^{2} &=2\left(y^{+}-y\right)^{T}\left(y-y^{\star}\right)+\left\|y^{+}-y\right\|_{2}^{2} \\&=-2 \rho G(y)^{T}\left(y-y^{\star}\right)+\rho^{2}\|G(y)\|_{2}^{2} \\&\leq-\rho(2-\rho) \| G(y)) \|_{2}^{2} \\&\leq-M \| G(y)) \|_{2}^{2}\end{aligned}
$$
其中 $M=\rho_{\min}(2-\rho_{\max})$。上式表明
$$
M \sum_{k=0}^{\infty}\left\|G\left(y_{k}\right)\right\|_{2}^{2} \leq\left\|y_{0}-y^{\star}\right\|_{2}^{2}, \quad \| G(y)\|_2\to 0
$$
还可以得到 $\| y_k-y^\star\|_2$ 是单调不增的，因此 $y_k$ 有界。

由于 $\| y_k-y^\star\|_2$ 单调不增，故极限 $\lim_{k\to \infty} \| y_k-y^\star\|_2$ 存在；又由于 $y_k$ 有界，故存在收敛子序列。

记 $\bar{y}_k$ 为一个收敛子序列，收敛值为 $\bar{y}$，根据 $G$ 的连续性有 $0=\lim _{k \rightarrow \infty} G\left(\bar{y}_{k}\right)=G(\bar{y})$，因此 $\bar{y}$ 是 $G$ 的l零点，且极限 $\lim_{k\to \infty} \| y_k-\bar{y}\|_2$ 存在。

接着需要证明唯一性，假设 $\bar{u},\bar{v}$ 是两个不同的极限点，收敛极限 $\lim_{k\to \infty} \| y_k-\bar{u}\|_2,\lim_{k\to \infty} \| y_k-\bar{v}\|_2$ 存在，因此
$$
\|\bar{u}-\bar{v}\|_{2}=\lim _{k \rightarrow \infty}\left\|y_{k}-\bar{u}\right\|_{2}=\lim _{k \rightarrow \infty}\left\|y_{k}-\bar{v}\right\|_{2}=0
$$
证毕。

