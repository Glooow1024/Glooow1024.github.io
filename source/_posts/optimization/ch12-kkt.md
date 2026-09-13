---
title: 凸优化笔记12：KKT 条件
date: 2020-03-26 14:34:22
tags:
  - 对偶原理
  - 拉格朗日函数
  - KKT条件
categories:
  - Convex Optimization
mathjax: true
---

上一小节讲了拉格朗日函数，可以把原始问题转化为对偶问题，并且对偶问题是凸的。我们还得到了弱对偶性和强对偶性的概念，并且提到了 Slater Condition 保证凸问题的强对偶性成立，并且给出了一些几何的直观解释。那么在这一节，我们将引出著名的 **KKT 条件**，它给出了最优解需要满足的必要条件，是求解优化问题最优解的一个重要方式。

<!--more-->

## 1. KKT 条件

我们首先回顾一下拉格朗日函数，考虑下面的优化问题
$$
\begin{aligned}
\text { minimize } \quad& f_{0}(x)\\
\text { subject to } \quad& f_{i}(x) \leq 0, \quad i=1, \ldots, m\\
&h_{i}(x)=0, \quad i=1, \ldots, p
\end{aligned}
$$
那么他的拉格朗日函数就是
$$
L(x,\lambda,\nu)=f_0(x)+\lambda^Tf(x)+\nu^Th(x)
$$
首先，我们看对偶函数
$$
g(\lambda,\nu)=\inf_{x\in\mathcal{D}}\left(f_0(x)+\lambda^Tf(x)+\nu^Th(x)\right)
$$
对偶问题实际上就是
$$
d^\star = \sup_{\lambda,\nu}g(\lambda,\nu)=\sup_{\lambda,\nu}\inf_x L(x,\lambda,\nu)
$$
然后我们再看原问题，由于 $\lambda\succeq0,f(x)\preceq0$，我们有
$$
f_0(x)=\sup_{\lambda,\nu}L(x,\lambda,\nu)
$$
原问题的最优解实际上就是
$$
p^\star=\inf_x f_0(x)= \inf_x \sup_{\lambda,\nu}L(x,\lambda,\nu)
$$
**弱对偶性** $p^\star \ge d^\star$ 实际上说的是什么呢？就是 **max-min 不等式**
$$
\inf_x \sup_{\lambda,\nu}L(x,\lambda,\nu) \ge \sup_{\lambda,\nu}\inf_x L(x,\lambda,\nu)
$$
**强对偶性**说的又是什么呢？就是上面能够取等号
$$
\inf_x \sup_{\lambda,\nu}L(x,\lambda,\nu) = \sup_{\lambda,\nu}\inf_x L(x,\lambda,\nu) = L({x}^\star,{\lambda}^\star,{\nu}^\star)
$$
实际上 ${x}^\star,{\lambda}^\star,{\nu}^\star$ 就是**拉格朗日函数的鞍点**！！！（数学家们真实太聪明了！！！妙啊！！！）那么也就是说**强对偶性成立等价于拉格朗日函数存在鞍点(在定义域内)**。

好，如果存在鞍点的话，我们怎么求解呢？还是看上面取等的式子
$$
\begin{aligned}
f_0({x}^\star) = g(\lambda^\star,\nu^\star) &= \inf_x \left( f_0(x)+\lambda^{\star T}f(x)+\nu^{\star T}h(x) \right) \\
& \le f_0(x^\star)+\lambda^{\star T}f(x^\star)+\nu^{\star T}h(x^\star) \\
& \le f_0(x^\star)
\end{aligned}
$$
这两个不等号必须要取到等号，而第一个不等号取等条件应为
$$
\nabla_x \left( f_0(x)+\lambda^{\star T}f(x)+\nu^{\star T}h(x) \right) =0
$$
第二个不等号取等条件为
$$
\lambda^\star_i f_i(x^\star)=0,\forall i
$$
同时，由于 ${x}^\star,{\lambda}^\star,{\nu}^\star$ 还必须位于定义域内，需要满足约束条件，因此上面的几个条件共同构成了 KKT 条件。

> **KKT 条件**
>
> 1. 原始约束 $f_i(x)\le0,i=1,...,m, \quad h_i(x)=0,i=1,...,p$
> 2. 对偶约束 $\lambda\succeq0$
> 3. 互补性条件(complementary slackness) $\lambda_i f_i(x)=0,i=1,...,m$
> 4. 梯度条件
>
> $$
> \nabla f_{0}(x)+\sum_{i=1}^{m} \lambda_{i} \nabla f_{i}(x)+\sum_{i=1}^{p} \nu_{i} \nabla h_{i}(x)=0
> $$

## 2. KKT 条件与凸问题

> **Remarks(重要结论)**
>
> 1. 前面推导没有任何凸函数的假设，因此不论是否为凸问题，**如果满足强对偶性，那么最优解一定满足 KKT 条件**。
> 2. 但是反过来不一定成立，也即 **KKT 条件的解不一定是最优解**，因为如果 $L(x,\lambda^\star,\nu^\star)$ 不是凸的，那么 $\nabla_x L=0$ 并不能保证 $g(\lambda^\star,\nu^\star)=\inf_x L(x,\lambda^\star,\nu^\star)\ne L(x^\star,\lambda^\star,\nu^\star)$，也即不能保证 ${x}^\star,{\lambda}^\star,{\nu}^\star$ 就是鞍点。

但是如果我们假设原问题为凸问题的话，那么 $L(x,\lambda^\star,\nu^\star)$ 就是一个凸函数，由梯度条件 $\nabla_x L=0$ 我们就能得到 $g(\lambda^\star,\nu^\star)=L(x^\star,\lambda^\star,\nu^\star)=\inf_x L(x,\lambda^\star,\nu^\star)$，另一方面根据互补性条件我们有此时 $f_0(x^\star)=L(x^\star,\lambda^\star,\nu^\star)$，因此我们可以得到一个结论

> **Remarks(重要结论)**：
>
> 1. 考虑原问题为凸的，那么若 KKT 条件有解 $\tilde{x},\tilde{\lambda},\tilde{\nu}$，则原问题一定满足强对偶性，且他们就对应原问题和对偶问题的最优解。
> 2. 但是需要注意的是，KKT 条件可能无解！此时就意味着原问题不满足强对偶性！

假如我们考虑上一节提到的 SCQ 条件，如果凸优化问题满足 SCQ 条件，则意味着强对偶性成立，则此时有结论

> **Remarks(重要结论)**：
>
> 如果 SCQ 满足，那么 $x$ 为最优解**当且仅当**存在 $\lambda,\nu$ 满足 KKT 条件！

***例子 1***：等式约束的二次优化问题 $P\in S_+^n$
$$
\begin{aligned}
\text { minimize } \quad& (1/2)x^TPx+q^Tx+r \\
\text { subject to } \quad& Ax=b
\end{aligned}
$$
那么经过简单计算就可以得到 KKT 条件为
$$
\left[\begin{array}{cc}
P & A^{T} \\
A & 0
\end{array}\right]\left[\begin{array}{l}
x^{\star} \\
\nu^{\star}
\end{array}\right]=\left[\begin{array}{c}
-q \\
b
\end{array}\right]
$$
***例子 2***：注水问题
$$
\begin{aligned}
&\text { minimize } \quad-\sum_{i=1}^{n} \log \left(\alpha_{i}+x_{i}\right)\\
&\text { subject to } \quad x \succeq 0, \quad \mathbf{1}^{T} x=1
\end{aligned}
$$
根据上面的结论，$x$ 是最优解当且仅当 $x\succeq0,\mathbf{1}^{T} x=1$，且存在 $\lambda,\nu$ 满足
$$
\lambda \succeq 0, \quad \lambda_{i} x_{i}=0, \quad \frac{1}{x_{i}+\alpha_{i}}+\lambda_{i}=\nu
$$
根据互补性条件 $\lambda_i x_i=0$ 分情况讨论可以得到

- 如果 $\nu<1/\alpha_i$：$\lambda_i=0,x_i=1/\nu-\alpha_i$
- 如果 $\nu\ge1/\alpha_i$：$\lambda_i=\nu-1/\alpha_i,x_i=0$

整理就可以得到 $\mathbf{1}^{T} x=\sum_i\max\{0,1/\nu-\alpha_i\}$，这个式子怎么理解呢？就像向一个池子里注水一样

![water filling](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/12-water.png)

## 3. 扰动与敏感性分析

现在我们再回到原始问题
$$
\begin{aligned}
\text { minimize } \quad& f_{0}(x)\\
\text { subject to } \quad& f_{i}(x) \leq 0, \quad i=1, \ldots, m\\
&h_{i}(x)=0, \quad i=1, \ldots, p
\end{aligned}
$$
我们引入了对偶函数 $g(\lambda,\nu)$，那这两个参数 $\lambda,\nu$ 有什么含义吗？假如我们把原问题放松一下
$$
\begin{aligned}
\text { minimize } \quad& f_{0}(x)\\
\text { subject to } \quad& f_{i}(x) \leq u_i, \quad i=1, \ldots, m\\
&h_{i}(x)=v_i, \quad i=1, \ldots, p
\end{aligned}
$$
记最优解为 $p^\star(u,v)=\min f_0(x)$，现在对偶问题变成了
$$
\begin{aligned}
\max \quad&  g(\lambda,\nu)-u^T\lambda -v^T\nu\\
\text{s.t.} \quad& \lambda\succeq0
\end{aligned}
$$
假如说原始对偶问题的最优解为 $\lambda^\star,\nu^\star$，松弛后的对偶问题最优解为 $\tilde{\lambda},\tilde{\nu}$，那么根据弱对偶性原理，有
$$
\begin{aligned}
p^\star(u,v) &\ge g(\tilde\lambda,\tilde\nu)-u^T\tilde\lambda -v^T\tilde\nu \\
&\ge g(\lambda^\star,\nu^\star)-u^{T}\lambda^\star -v^{T}\nu^\star \\
&= p^\star(0,0) - u^{T}\lambda^\star -v^{T}\nu^\star
\end{aligned}
$$
这像不像关于 $u,v$ 的一阶近似？太像了！实际上，我们有
$$
\lambda_{i}^{\star}=-\frac{\partial p^{\star}(0,0)}{\partial u_{i}}, \quad \nu_{i}^{\star}=-\frac{\partial p^{\star}(0,0)}{\partial v_{i}}
$$
![sensitivity](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/12-sensitivity.PNG)

## 4. Reformulation

前面将凸优化问题的时候，我们提到了Reformulation的几个方法来简化原始问题，比如消去等式约束，添加等式约束，添加松弛变量，epigraph等等。现在当我们学习了对偶问题，再来重新看一下这些方法。

### 4.1 引入等式约束

***例子 1***：考虑无约束优化问题 $\min f(Ax+b)$，他的对偶问题跟原问题是一样的。如果我们引入等式约束，原问题和对偶问题变为
$$
\begin{aligned}
\text{minimize} \quad& f_{0}(y) \quad \\
\text{subject to} \quad& A x+b-y=0
\end{aligned}
\quad\qquad
\begin{aligned}
\text{minimize} \quad& b^{T} \nu-f_{0}^{*}(\nu) \\
\text{subject to} \quad& A^{T} \nu=0
\end{aligned}
$$
***例子 2***：考虑无约束优化 $\min \Vert Ax-b\Vert$，类似的引入等式约束后，对偶问题变为
$$
\begin{aligned}
\text{minimize} \quad& b^{T} \nu \\
\text{subject to} \quad& A^{T} \nu=0,\quad \Vert\nu\Vert_*\le1
\end{aligned}
$$

### 4.2 显示约束与隐式约束的相互转化

***例子 3***：考虑原问题如下，可以看出来对偶问题非常复杂
$$
\begin{aligned}
\text{minimize} \quad& c^{T} x \\
\text{subject to} \quad& A x=b \\
\quad& -1 \preceq x \preceq 1
\end{aligned}

\begin{aligned}
\text{maximize} \quad& -b^{T} \nu-\mathbf{1}^{T} \lambda_{1}-\mathbf{1}^{T} \lambda_{2} \\
\text{subject to} \quad& c+A^{T} \nu+\lambda_{1}-\lambda_{2}=0 \\
\quad& \lambda_{1} \succeq 0, \quad \lambda_{2} \succeq 0 
\end{aligned}
$$
如果我们原问题的不等式约束条件转化为隐式约束，则有
$$
\begin{aligned}
\text{minimize} \quad& f_{0}(x)=\left\{\begin{array}{ll}c^{T} x & \Vert x\Vert_\infty \preceq 1 \\ \infty & \text { otherwise }\end{array}\right. \\
\text{subject to} \quad& A x=b
\end{aligned}
$$
然后对偶问题就可以转化为无约束优化问题
$$
\text{maximize} -b^T\nu-\Vert A^T\nu +c\Vert_1
$$

### 4.3 转化目标函数与约束函数

***例子 4***：还考虑上面提到的无约束优化问题 $\min \Vert Ax-b\Vert$，我们可以把目标函数平方一下，得到
$$
\begin{aligned}
\text{minimize} \quad& (1/2)\Vert y\Vert^2 \\
\text{subject to} \quad& Ax-b=y
\end{aligned}
$$
然后对偶问题就可以转化为
$$
\begin{aligned}
\text{minimize} \quad& (1/2)\Vert \nu\Vert_*^2+ b^T\nu \\
\text{subject to} \quad& A^T\nu=0
\end{aligned}
$$