---
title: 凸优化笔记20：对偶近似点梯度下降
date: 2020-04-23 18:35:56
tags:
  - PG 算法
  - 拉格朗日函数
  - 近似点算子
  - 共轭函数
  - ALM
categories:
  - Convex Optimization
mathjax: true
---

前面讲了梯度下降、次梯度下降、近似点梯度下降方法并分析了收敛性。一开始我们还讲了对偶原理，那么如果原问题比较难求解的时候，我们可不可以转化为对偶问题并应用梯度法求解呢？当然可以，不过有一个问题就是对偶函数的梯度或者次梯度怎么计算呢？这就是这一节要关注的问题。

首先一个问题是哪些形式的问题，其对偶问题相比于原问题更简单呢？可能有很多种，这一节主要关注一种：**线性等式/不等式约束的优化问题**。之所以考虑此类问题是因为如果有线性的等式/不等式约束，应用 Lagrange 对偶原理之后，我们可以自然的用原函数的**共轭函数**来表示其**对偶问题**，而共轭函数的次梯度是容易求解的，因为我们可以用 **Legendre transformation**。那么接下来就是详细的原理和例子了。

<!--more-->

## 1. 对偶分解原理

假如我们考虑如下无约束优化问题，通过添加线性等式约束以及对偶原理可以容易获得对偶问题的形式
$$
\begin{aligned}
(P)\quad\min \ & f(x)+g(Ax) \\
(D)\quad\max \ & -g^\star(z)-f^\star(-A^Tz)
\end{aligned}
$$
如果我们现在想对偶问题应用梯度下降方法，一个关键的问题就是如何求解 $f^\star,g^\star$ 的梯度/次梯度？会议一下前面讲共轭函数的时候提到了一些性质：

> 关于共轭函数有以下性质
>
> 1. 若 $f$ 为凸的且是闭的($\text{epi }f$ 为闭集)，则 $f^{**}=f$ (可以联系上面提到一系列支撑超平面)
> 2. (Fenchel's inequality) $f(x)+f^*(y)\ge x^Ty$，这可以类比均值不等式
> 3. (**Legendre transform**)如果 $f$ 且为**凸的、闭的**，设 $x^*=\arg\max\{y^Tx-f(x)\}$，那么有 $x^*\in\partial f^*(y)\iff y\in\partial f(x^*)$。这可以用来求极值，比如 $\min f(x)\Longrightarrow 0\in\partial f(x)\iff x\in\partial f^*(0)$

所以只需要找到
$$
\hat{x}=\arg\max_x -z^TAx-f(x) = \arg\min_x z^TAx+f(x)
$$
就可以有 $\hat{x}\in\partial f^\star(-A^Tz)$。如果函数 $f$ 为**严格凸函数**，意味着上面的 $\hat{x}$ 唯一，也说明 $\partial f^\star=\nabla f^\star$；如果条件加强，$f$ 为$\mu$-强凸函数那么就有
$$
\left\|\nabla f^{*}(y)-\nabla f^{*}\left(y^{\prime}\right)\right\| \leq \frac{1}{\mu}\left\|y-y^{\prime}\right\|_{*} \quad \text { for all } y, y^{\prime}
$$
也就是说共轭函数的梯度是 Lipschitz continuous 的，也即共轭函数 $f^\star$ 是 $1/\mu$-smooth 的，因此我们也能证明对偶问题下用梯度方法的收敛性。

***例子***：现在我们把上面的 $g$ 换成一个指示函数，也即表示一个线性等式约束
$$
\begin{aligned}
(P)\quad\text{minimize } \quad& f(x) \\
\text{subject to } \quad& Ax=b \\
(D)\quad\text{minimize } \quad& -b^Tz-f^\star(-A^Tz)
\end{aligned}
$$
怎么做梯度下降呢？
$$
\begin{aligned}
\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+z^{T} A x\right) \\
z^{+} &=z+t(A \hat{x}-b)
\end{aligned}
$$
上面第一步就是为了求解 $f^\star$ 的次梯度，第二部就是对 $z$ 进行梯度上升。我们再来观察一下 Lagrange 函数
$$
\begin{aligned}
L(x,z)&=f(x)+z^T(Ax-b) \\
&=-b^Tz+(f(x)+z^TAx)
\end{aligned}
$$
我么前面说过最优解实际上是拉格朗日函数的鞍点，也就是关于 $x$ 的极小值点，关于 $z$ 的极大值点。而这里我们要做的两部就分别是对 $x$ 求一个极小值点 $\hat{x}$，然后对 $z$ 并没有求极大值点，而是做了一个梯度上升。

好了，对偶问题的梯度下降方法原理就这些，但是标题里面“对偶分解”还有一个“**分解**”是什么意思呢？我们说如果原问题比较复杂就可以考虑解对偶问题，那么具体是哪一种问题呢？看个例子
$$
\begin{aligned}
\text{minimize } \quad& f_1(x_1)+f_2(x_2) \\
\text{subject to } \quad& A_1x_1+A_2x_2\preceq b \\
\end{aligned}
$$
这个问题有什么特点呢？目标函数实际上是由两个不相关的函数 $f_1(x_1),f_2(x_2)$ 求和得到的，注意不仅是 $f_1,f_2$ 不同，他们的自变量 $x_1,x_2$ 也是相互独立的，也即是说假如没有这个约束条件，我们完全可以分解为两个独立的最小化问题分别求解。但是现在**由于这个约束条件，这两个问题耦合在一起了**，这就有点麻烦了。那么在对偶问题中能不能解耦合呢？好消息是可以！他们的对偶问题可以写为
$$
\begin{aligned}
\text{minimize } \quad& -f_1^\star(-A_1^Tz)-f_2^\star(-A_2^Tz)-b^Tz \\
\text{subject to } \quad& z\succeq 0 \\
\end{aligned}
$$
这个时候我们只需要分别求解 $f_1^\star,f_2^\star$ 的次梯度，这两个是可以并行计算的，即下面的 $\hat{x}_1,\hat{x}_2$ 可以并行计算
$$
\begin{aligned}
&\hat{x}_{j}=\underset{x_{j}}{\operatorname{argmin}}\left(f_{j}\left(x_{j}\right)+z^{T} A_{j} x_{j}\right) \quad \text { for } j=1,2\\
&z^{+}=\left(z+t\left(A_{1} \hat{x}_{1}+A_{2} \hat{x}_{2}-b\right)\right)_{+}
\end{aligned}
$$
所以每次迭代我们都可以先并行计算 $\hat{x}_j$，然后把他们传给中心节点，中心节点再对 $z$ 计算梯度上升。需要注意的一点是这里对 $z^+$ 只取正值，这是因为有 $z\succeq 0$ 的约束，从另一个角度理解也是向 $C=\{z|z\succeq0\}$ 做了投影（回忆上一节的近似点算子与投影的关系，以及近似点梯度下降实际上就是先算梯度再做投影）。

***例子  1***：来看一个二次优化的例子，假设其中的 $P_j\succ0$
$$
\begin{array}{ll}
\text { minimize } & \sum_{j=1}^{r}\left(\frac{1}{2} x_{j}^{T} P_{j} x_{j}+q_{j}^{T} x_{j}\right) \\
\text { subject to } & B_{j} x_{j} \preceq d_{j}, \quad j=1, \ldots, r \\
& \sum_{j=1}^{r} A_{j} x_{j} \preceq b
\end{array}
$$
这里约束条件比较多，我们可以转化一下，考虑 $f_j(x_j)=\frac{1}{2} x_{j}^{T} P_{j} x_{j}+q_{j}^{T} x_{j}$，定义域为 $\{x_j|B_jx_j\preceq d_j\}$。对偶问题就变成了
$$
\begin{aligned}
\text{minimize } \quad& -b^Tz-\sum_{j=1}^r f_j^\star(-A_j^Tz) \\
\text{subject to } \quad& z\succeq 0 \\
\end{aligned}
$$
为了保证梯度下降方法的收敛性，还需要验证目标函数的 Lipschitz smooth 性质，考虑 $h(z)=\sum_j f_j^\star(-A_j^Tz)$，有
$$
\left\|\nabla h(z)-\nabla h\left(z^{\prime}\right)\right\|_{2} \leq \frac{\|A\|_{2}^{2}}{\min _{j} \lambda_{\min }\left(P_{j}\right)}\left\|z-z^{\prime}\right\|_{2}
$$
其中 $A=[\begin{array}{ccc}A_1&\cdots&A_r\end{array}]$。那么怎么求 $\hat{x}_j=\partial f_j^\star(-A_j^Tz)$ 呢？就是求解下面的优化问题
$$
\begin{array}{ll}
\text { minimize } & \frac{1}{2} x_{j}^{T} P_{j} x_{j}+(q_{j}+A_j^Tz)^T x_{j} \\
\text { subject to } & B_{j} x_{j} \preceq d_{j}
\end{array}
$$
***例子 2***：网络优化问题
$$
\begin{aligned}
(P) \quad \text { maximize } \quad& \sum_{j=1}^{n} U_{j}\left(x_{j}\right)\\
\text { subject to } \quad& R x \leq c\\
(D) \quad \text { minimize } \quad& c^{T} z+\sum_{j=1}^{n}\left(-U_{j}\right)^{*}\left(-r_{j}^{T} z\right)\\
\text { subject to } \quad& z \geq 0
\end{aligned}
$$
只需要将 $R$ 的各列拆分开就行了。

## 2. 对偶近似点梯度方法

这节的一开始我们考虑的问题是
$$
\begin{aligned}
(P)\quad\min \ & f(x)+g(Ax) \\
(D)\quad\min \ & g^\star(z)+f^\star(-A^Tz)
\end{aligned}
$$
不过举得几个例子中都没有见到 $g$ 的影子，这是因为我们都加了线性等式/不等式约束，也就等价于 $g=\delta_C(x)$ 是一个指示函数的形式。那如果考虑一般的(不光滑的) $g$ 呢？很简单，我们本质上还是在做梯度下降(上升)，如果函数不光滑，就可以用上一篇文章的近似点梯度方法，还记得近似点梯度下降的公式吗？
$$
z^{+}=\operatorname{prox}_{t g^{*}}\left(z+t A \nabla f^{*}\left(-A^{T} z\right)\right)
$$
实际上刚刚举得几个例子也都是在做近似点梯度下降，只不过对于指示函数的近似点就是在做投影，比较简单。所以对上面的问题求解实际上就是两步：
$$
\begin{aligned}
\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+z^{T} A x\right) \\
z^{+} &=\operatorname{prox}_{t g^{*}}(z+t A \hat{x})
\end{aligned}
$$
如果有时候 $g^\star$ 的近似点不好计算，也可以利用 Moreau 分解(参见上上一节近似点算子)，那么可以计算
$$
z^{+} =z+tA\hat{x}-t\operatorname{prox}_{t^{-1} g}(t^{-1}z+A \hat{x})
$$
这个式子其实可以跟增广拉格朗日方法(后面会讲)联系起来。我们可以令 $\hat{y}$ 为
$$
\begin{aligned}
\hat{y} &=\operatorname{prox}_{t^{-1} g}\left(t^{-1} z+A \hat{x}\right) \\
&=\underset{y}{\operatorname{argmin}}\left(g(y)+\frac{t}{2}\left\|A \hat{x}-t^{-1} z-y\right\|_{2}^{2}\right) \\
&=\underset{y}{\operatorname{argmin}}\left(g(y)+z^{T}(A \hat{x}-y)+\frac{t}{2}\|A \hat{x}-y\|_{2}^{2}\right)
\end{aligned}
$$
那么就有 $z^{+}=z+t(A\hat{x}-\hat{y})$，所以这里 $\hat{y}$ 一定程度上可以理解为 $g^\star(z)$ 的次梯度。把前面的分析综合起来，我们梯度下降的每次迭代过程实际上可以由下面 3 个步骤组成
$$
\begin{aligned}
\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+z^{T} A x\right) \\
\hat{y} &=\underset{y}{\operatorname{argmin}}\left(g(y)-z^{T} y+\frac{t}{2}\|A \hat{x}-y\|_{2}^{2}\right) \\
z^{+} &=z+t(A \hat{x}-\hat{y})
\end{aligned}
$$
这个时候可以跟**增广拉格朗日方法(augmented Lagrangian method)**比较一下，ALM 的计算公式为
$$
(\hat{x}, \hat{y})=\underset{x, y}{\operatorname{argmin}}\left(f(x)+g(y)+z^{T}(A x-y)+\frac{t}{2}\|A x-y\|_{2}^{2}\right)
$$
首先 ALM 要优化的这个增广拉格朗日函数函数是在普通拉格朗日函数的基础上加了一个最后的二阶项，然后对增广拉函数优化 $x,y$。而前面的对偶近似梯度方法是怎么做呢？观察增广拉函数实际上可以分解为两项
$$
L(x,y,z)=\left(f(x)+z^{T}A x\right)+\left(g(y)-z^Ty+\frac{t}{2}\|A x-y\|_{2}^{2}\right)
$$
而对偶近似梯度方法实际上就是先优化第一项，只考虑 $x$，计算得到 $\hat{x}$ 以后再代入到第二项单独优化 $y$ 得到 $\hat{y}$，最后对 $z$ 做梯度上升！巧妙不巧妙！显然对偶近似梯度方法计算起来更简单，但是也有代价，那就是 ALM 并不要求函数 $f$ 是强凸的，而对偶近似梯度方法则要求 $f$ 为强凸的。

基本的理论就完了，下面主要是一些例子。

***例子 1***：$g$ 为范数正则项
$$
\begin{aligned}
(P)\quad\text{minimize}\quad& f(x)+\|A x-b\| \\
(D)\quad\text{maximize} \quad& -b^{T} z-f^{*}\left(-A^{T} z\right)\\
\text{subject to} \quad& \|z\|_{*} \leq 1
\end{aligned}
$$
我们可以取 $g(y)=\Vert y-b\Vert$
$$
g^{*}(z)=\left\{\begin{array}{ll}
b^{T} z & \|z\|_{*} \leq 1 \\
+\infty & \text { otherwise }
\end{array} \quad \operatorname{prox}_{t g *}(z)=P_{C}(z-t b)\right.
$$
对偶近似梯度下降方法每次迭代过程为
$$
\begin{aligned}
\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+z^{T} A x\right) \\
z^{+} &=P_{C}(z+t(A \hat{x}-b))
\end{aligned}
$$
***例子 2(重要)***：前面都只有 1 个 $g$，这次考虑 $g$ 为多个 $g_i$ 求和
$$
\begin{aligned}
(P) \quad \text { minimize } \quad& f(x)+\sum_{i=1}^{p}\left\|B_{i} x\right\|_{2}\\
(D) \quad \text { maximize } \quad& -f^{*}\left(-B_{1}^{T} z_{1}-\cdots-B_{p}^{T} z_{p}\right)\\
\text { subject to }\quad& \left\|z_{i}\right\|_{2} \leq 1, \quad i=1, \ldots, p
\end{aligned}
$$
这个推导就有点麻烦了，我们考虑一般的情况 $g(x)=g_1(B_1x)+...+g_p(B_px)$，那么可以取 $B=[B_1;...;B_p]$(排成一列)，$g(x)=\hat{g}(Bx)$，那么原问题实际上等价于 
$$
\begin{aligned}
\text{minimize } \quad& f(x)+\hat{g}(y) \\
\text{subject to } \quad& y=Bx \\
\end{aligned}
$$
拉格朗日函数为 $L(x,y,z)=f(x)+\sum_i g_i(y_i) + \sum_i z_i^T(B_ix-y_i)$，对偶问题就变成了
$$
\text{maximize}\quad -f^\star(-\sum_i B_i^Tz_i) - \sum_i g_i^\star(z_i)
$$
注意到对偶问题中 $g^\star(z)=\sum_i g^\star_i(z_i)$，利用近似点算子公式就可以得到
$$
\operatorname{prox}_{tg^\star}(x)=\left[\begin{array}{c}\operatorname{prox}_{tg^\star_1}(x_1)\\ \vdots \\ \operatorname{prox}_{tg^\star_p}(x_p) \end{array}\right]
$$
所以我们的 $z_i^+$ 之间的计算是互不相关的，可以并行进行，也即下面的式子中 $\hat{x}$此时不能并行计算了，但是 $z_i^+$可以分别计算
$$
\begin{aligned}
\hat{x} &=\underset{x}{\operatorname{argmin}}\left(f(x)+\left(\sum_{i=1}^{p} B_{i}^{T} z_{i}\right)^{T} x\right) \\
z_{i}^{+} &=P_{C_{i}}\left(z_{i}+t B_{i} \hat{x}\right), \quad i=1, \ldots, p
\end{aligned}
$$
注意第一部分当中我们考虑 $f_1(x_1)+f_2(x_2)$ 形式的优化问题，这使得对偶问题可以分解为 $\sum_i f^\star(-A_i^Tz)$，可以并行计算 $\hat{x}_i$，而这里我们考虑 $\sum_i g(x)$ 则对偶问题可以表示为 $\sum_i g^\star(z_i)$，这使得计算近似点梯度的时候可以对 $z_i^+$ 并行计算，非常的对称！