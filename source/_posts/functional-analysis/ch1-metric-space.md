---
title: 泛函分析笔记1：度量空间
date: 2020-10-18 21:43:17
tags:
  - 度量空间
  - 完备空间
  - 开集
  - 可分性
  - 柯西列
  - 连续映射
  - Banach不动点定理
categories:
  - Functional Analysis
mathjax: true
---

这一章节研究度量空间的基本结构，在开始前，我们需要思考几个问题：

1. 什么是度量空间？
2. 我们为什么要首先学习度量空间呢？

在这篇笔记的最后再来回答这个问题。

<!--more-->

## 1. 度量空间定义

对于工科生来说，“空间”这个概念还是比较模糊的，但是我本人在学习数学的过程中发现很多数学的分支学科都是对于某个空间进行研究，比如欧氏空间、内积空间、测度空间、拓扑空间、希尔伯特空间，以及这门课要学习的度量空间等等。

个人觉得，定义空间的同时往往伴随着相应的某种运算的定义，也就是说我们研究空间的时候，主要还是为了研究某些运算，为了保证运算前后的结果都有比较好的性质（或者说便于处理），我们才抽象出了对应的空间的概念。回到这门课，泛函分析里面将要遇到的空间都是度量空间，因此我们首先需要知道他的定义。

**“度量”实际上指的就是抽象的“距离”**，比如两个点的距离、两个向量的距离、两个无穷长序列的距离、两个连续函数的距离。尽管点、向量、无穷长序列、连续函数的形式差别很大，但是如果我们都把他们抽象成为高维空间中的一个点，我们可以用相似的方式定义距离（也就是度量）的概念。

这样一来，由于**大多数时候我们关心的是两个高维点之间的相对距离，而不是他们本身的绝对坐标**（比如我们研究数列收敛性 $x_n\to x$ 的时候，只需要分析 $|x_n-x|\to 0$ 是否成立，而不太关心 $x$ 具体是什么），因此抽象出距离这个概念之后，我们就可以把点、向量、序列、连续函数都统一的看待，用统一的方法和理论处理相似的问题。

首先，我们可以抽象出一个集合 $X$。$X$ 可以是一个实数点集，比如 $X=\mathbb{R}$；也可以是一个多维实空间 $X=\mathbb{R}^n$，他的元素就是向量；也可以是连续函数集 $X=C[a,b]$，表示区间 $[a,b]$ 上的所有连续函数构成的集合。

现在问题的关键就是**如何定义度量**？当然不能随意定义，我们必须要他满足一定条件，才能称之为度量：

1. $\forall x,y\in X, d(x,y)\ge0$;
2. $x,y\in X, d(x,y)=0 \iff x=y$
3. $\forall x,y\in X, d(x,y)=d(y,x)$
4. $\forall x,y,z\in X, d(x,y)\le d(x,z)+d(z,y)$

根据定义可以看出来，度量就是一种距离，称 $(X,d)$ 为**度量空间**（或**距离空间**）。下面给出一些度量空间的例子，要证明是否是度量只需要验证几条定义即可。

**注**：后面的内容基本都默认我们是在度量空间 $(X,d)$ 中讨论，为了书写省劲，一般都没有明确写出来“存在度量空间 $(X,d)$”，但是需要注意我们是在度量空间中讨论的。

***例子 1(离散度量)***：定义
$$
d(x,y) = \begin{cases}
0, & x=y \\
1, & x\ne y
\end{cases}
$$
***例子 2***：记 $X=\mathbb{K}$，其中我们记 $\mathbb{K}=\mathbb{R} \text{ or } \mathbb{C}$（后面的笔记也都保持这个习惯），$d(x,y)=|x-y|$，那么 $(X,d)$ 是一个度量空间。

***例子 3***：$X=\mathbb{K}^n, d_p(\boldsymbol{x,y})=\|\boldsymbol{x-y}\|_p = (\sum_i |x_i-y_i|^p)^{1/p}$，对于 $1\le p \le \infty$，$(X,d_p)$ 是度量空间。

***例子 4***：$X=\ell^p,d_p(\boldsymbol{x,y})=\|\boldsymbol{x-y}\|_p$，对于 $1\le p\le \infty$，$(\ell^p,d_p)$ 是度量空间。其中
$$
\ell^p = \left\{(x_n)_{n\ge1},\ x_n\in\mathbb{K},\ \exists c\ge0,\ |x_n|\le c  \right\}
$$
这个在证明的过程中需要证明 $\forall x,y\in\ell^p$，都有 $x+y \in \ell^p$。

例子3、4的证明过程中还需要用到两个比较重要的不等式：Hölder 不等式和 Minkowski 不等式。

> **Hölder不等式**：$\forall 1\le p,q\le\infty, \frac{1}{p}+\frac{1}{q}=1,\forall x,y\in\mathbb{R}^n$，有
> $$
> \sum_{i=1}^{n}|x_iy_i| \le \|x\|_p \|y\|_q
> $$
> 当 $p=q=2$ 的时候，上面的式子就退化成 Cauchy-Schwarz 不等式。
>
> **证明**：可以取 $u(t)=t^{p-1},t\ge0$，反函数就有 $t=u^{\frac{1}{p-1}}=u^{q-1},u\ge0$。考虑可能有下面两种情况：
>
> ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/1-holder.png)
>
> 以第一种情况为例，$\int_0^\alpha t^{p-1}dt + \int_0^\beta u^{q-1}du = \frac{\alpha^p}{p}+\frac{\beta^q}{q} \ge \alpha\beta$，第二种情况也有相同的结果。我们可以首先假设 $\|x\|_p = \|y\|_q=1$，因而就有
> $$
> |x_iy_i| \le \frac{|x_i|^p}{p} + \frac{|y_i|^q}{q} \\
> \Longrightarrow \sum |x_iy_i| \le 1 = \|x\|_p \|y\|_q
> $$
> 如果 $\|x\|_p \ne 1$，那么我们可以取 $x' = x/\|x\|_p$，代入上面的情况就能得到 Hölder 不等式了。证毕。
>
> **Minkowski 不等式**：$\forall x,y\in\ell^p, p\ge1$，都有
> $$
> \|x+y\|_p \le \|x\|_p + \|y\|_p .
> $$
> **证明**：
>
> 证毕。

对于空间 $\ell^p$ 还有如下有趣的**性质**：

- 若 $1\le p\le q$，则 $\ell^p \subset \ell^q$
- $\lim_{p\downarrow 1} \|x\|_p = \|x\|_1$
- $\lim_{p\uparrow 1} \|x\|_p = \|x\|_\infty$

***例子 5(无穷维向量)***：定义 $S=\{(x_n)_{n\ge1}, x_n\in\mathbb{K}\}$，定义如下度量
$$
d(x,y) = \sum_{n=1}^\infty\frac{1}{2^n}\frac{|x_n-y_n|}{1+|x_n-y_n|} < \infty
$$
则 $(S,d)$ 是度量空间。

***例子 6(连续函数)***：$X=C[a,b],\ d_p(x,y) = \left(\int_a^b |x(t)-y(t)|^pdt\right)^{1/p},\ 1\le p\le\infty$，则 $(X,d)$ 是度量空间。



## 2. 开集

上面一小节分析了度量空间 $(X,d)$ 中度量 $d$ 的定义，这一部分研究一下集合 $X$ 的性质。我们主要从**开集和闭集**这个角度来分析集合的结构。

需要注意的是在实空间 $\mathbb{R}$ 当中我们对开集（开区间）都比较熟悉了，但是还有很多其他很复杂的集合，比如上面提到的连续函数构成的集合 $C[a,b]$，这种情况下什么是**开集**呢？这个时候我们就需要抽象出“开集”这个概念最为本质的性质了。

### 2.1 开集的定义

**定义**：开球 $B(x_0,\delta)=\{x\in X,\ d(x,x_0)<\delta\}$，闭球 $\bar{B}(x_0,\delta)=\{x\in X,\ d(x,x_0)\le\delta\}$，球面 $S(x_0,\delta)=\{x\in X,\ d(x,x_0)=\delta\}.$

**定义**：$(X,d),M\subset X$，称 $x_0\in M$ 为 $M$ 的**内点**，若 $\exists \delta>0, B(x_0,\delta)\subset M$，$M$ 的所有内点的集合称为 $M$ 的内部，记为 $\mathring{M}$。

**定义**：$M$ 为**开集** $\iff M=\mathring{M} \iff \forall x\in M,\ \exists \delta>0,\ B(x,\delta)\subset M$。

**定义**：$F\subset X$ 为**闭集** $\iff F^c=X\setminus F$ 为开集。

**定义**：**闭包** $\bar{M}=\{x\in X,\ \forall \delta>0,\ B(x,\delta)\cap M\ne \varnothing \}.$

**注 1**：有的集合可能既不是开集，也不是闭集！比如实空间 $\mathbb{R}$ 中区间 $[0,1)$。

**注 2**：但是假如现在考虑的不是实空间，而是 $X=[0,+\infty)$，那么 $[0,1)$ 就是开集！判断是否是开集还是要根据定义！

***例子 1(离散度量空间)***：$(X,d)$ 为离散度量空间，那么任意的 $M\subset X$ 都是既开又闭的集合。因为我们可以取 $\forall x\in M,\ B(x,1/2)=\{x\}\subset M$。

### 2.2 开集的性质

**命题**：$\mathring{M}$ 为包含在 $M$ 中的最大开集。

**证明**：分为三个过程：1）$\mathring{M}\subset M$；**2）$\mathring{M}$ 为开集；**3）$\mathring{M}$ 最大。注意不要忘了第 2) 部分，细节略。

**定理**：$(X,d)$，则

- $X,\varnothing$ 为开集
- $(G_i)_{i\in I}$ 为一族开集，则 $\bigcup_{i\in I}G_i$ 为开集（**开集无限并**仍然是开集）；
- $G_1,...,G_n$ 为开集，则 $\bigcap_{i=1}^n G_i$ 为开集（**开集有限交**仍然是开集）；

证明：略。

**定理**：$(X,d)$，则

- $X,\varnothing$ 为闭集（注意 $X,\varnothing$ 既开又闭）
- $(F_i)_{i\in I}$ 为一族开集，则 $\bigcap_{i\in I}F_i$ 为开集（**闭集无限交**仍然是闭集）；
- $F_1,...,F_n$ 为开集，则 $\bigcup_{i=1}^n F_i$ 为开集（**闭集有限并**仍然是闭集）；

证明：略。

**性质**：闭包 $\bar{M}$ 为闭集，且 $\bar{M}$ 为包含 $M$ 的最小闭集。

**推论**：$(X,d), M\subset X$，$M$ 为闭集 $\iff M=\bar{M}.$

证明：略。

> **拓扑空间**
>
> 拓扑的定义是：给集合 $X$ 指定拓扑，就是指定集合 $X$ 中哪些子集是开集，指定的方式需要满足：
>
> 1. $R,\varnothing$ 是开集；
> 2. 开集的有限交仍然是开集；
> 3. 开集的任意并仍然是开集。
>
> $X$ 上的**拓扑** $\mathcal{T}$ 是 $X$ 的子集族，满足上述的条件。定义了拓扑 $\mathcal{T}$ 的集合 $X$ 称为**拓扑空间**。对于拓扑空间 $(X,\mathcal{T})$ 有子集 $\mathcal{O}$，若 $\mathcal{O}\in \mathcal{T}$，则称 $\mathcal{O}$ 为开集。
>
> **注1**：先有拓扑 $\mathcal{T}$，然后如果 $X$ 的子集 $\mathcal{O}\in \mathcal{T}$，才有 $\mathcal{O}$ 是开集的说法。
>
> **注2**：拓扑空间跟度量空间类似，首先定义了一种运算，比如度量空间是需要定义度量，拓扑空间是需要定义对开集封闭的运算（有限交、任意并），其中的元素对这些运算封闭，然后才有空间的概念.

### 2.3 稠密与可分

**定义**：称 $M\subset X$ 是 $X$ 的**稠密子集**，若 $\bar{M}=X$。换一种表述方式，也就是说 $\forall x\in X,\forall \delta > 0, B(x,\delta)\cap M\ne \varnothing$。

***例子 1***：$(\mathbb{R},d)$，其中 $d(x,y)=|x-y|$，则 $\bar{\mathbb{Q}}=\mathbb{R}.$

***例子 2***：$(\mathbb{C},d)$，其中 $d(x,y)=|x-y|$，则 $\overline{\mathbb{Q}+i\mathbb{Q}}=\mathbb{C}.$

***例子 3***：$(\mathbb{R}^n,d_2)$，则 $\overline{\mathbb{Q}^n}=\mathbb{R}^n.$

***例子 4***：对于 $1\le p\le \infty$，定义 $M=\{(x_n)_{n\ge1},\ x_n\in\mathbb{Q},\exists N,\forall n\ge N, x_n=0 \}$，那么对于度量 $d_p$，有 $\bar{M}=\ell^p.$

**性质**：对于两个度量 $d_1,d_2$，如果存在 $c_1,c_2>0$，对 $\forall x,y\in X$，都有 $c_1d_1(x,y)\le d_2(x,y)\le c_2 d_1(x,y)$，也即这两个度量相互控制，那么对任意 $M\subset X$，有 $M$ 的**闭包相同**，**内部也相同**。

***例子 5***：对于离散度量空间，$X$ 的稠密子集只有 $X$ 本身。

***例子 6***：$X=C[a,b]$，$d_\infty(x,y)=\max_{t\in[a,b]} |x(t)-y(t)|$，因此 $M=\{多项式 p(t)=a_0+a_1t+\cdots+a_Nt^N,a_i\in\mathbb{Q}\}$，则 $\bar{M}=X.$

**定义**：称度量空间 $(X,d)$ 是**可分**的，若 $\exists M$ 为至多可数集（有限集或者可数集），并且 $\bar{M}=X.$

这里先介绍几个关于**可数集**的性质：

- 若 $X_1,...,X_n$ 可数，则 $X_1\times \cdots \times X_n$ 可数
- **可数个**可数集的**并集**仍然是可数集

***例子 1***：$\{0,1\}^{\mathbb{N}}$ **不是可数集**！其中 $\mathbb{N}$ 为自然数集。

***例子 2***：$(\mathbb{K}^n, d_p)$ 可分。

***例子 3***：$(\ell^p,d_p)$ 可分，$M$ 与上面例子 4 的定义相同。

***例子 4***：$(C[a,b],d_\infty)$ 可分，$M$ 与上面例子 6 的定义相同。

***例子 5***：$(\ell^\infty,d_\infty)$ **不可分**。

*证明*：反证法。参考课本 P14，略。



## 3. 收敛性与完备性

这一部分则开始考虑 $X$ 中的元素序列，以及序列的极限是否存在、极限是什么的问题。之所以考虑序列这件事情，是因为我们实际中处理问题的时候往往是用序列去逼近一个元素，序列当中的每个元素可能是简单的，二最后去逼近的这个元素往往是不太显然或者比较复杂的东西。这样我们只需要证明序列中的元素满足某些性质，就能证明最后的极限具有某些特殊性质，更容易处理。

### 3.1 序列收敛性

我们对**序列收敛性**的定义是对于 $x_n,x\in\mathbb{R}$，称 $x_n$ 收敛到 $x$，记为 $x_n\to x$ ($\lim_{n\to\infty} x_n=x$)，若 $\forall \varepsilon> 0,\exists N,\ \forall n\ge N$，都有 $|x_n-x|<\varepsilon$。换一种表述方式就是在度量空间 $(X,d)$ 中，$d(x_n,x)\to 0$。

**注**：需要注意的是只有 $x\in X$，我们才能说 $x_n\to x$。例如取 $X=(0,1),x_n=\frac{1}{n+1}$，那么 $x_n$ 在 $X$ 中**不收敛**。

**命题**：若 $x_n\to x$，则 $\{x_n\}_{n\ge1}$ 为有界集合，且 $x$ 唯一。

**定理**：度量空间 $(X,d)$，有 $M\subset X$，那么

- $x\in \bar{M} \iff \exists x_n\in M,\ x_n\to x$；
- $M$ 为**闭集** $\iff \forall x_n\in M$，设 $x_n\to x\in X$，则 $x\in M$（即**闭集对极限封闭**）。

证明：第一条应用定义，第二条应用第一条的结论。细节略。

### 3.2 柯西列与完备性

**定义**：$x_n\in X$，称 $x_n$ 为**柯西列**，若 $\forall \varepsilon > 0,\exists N, \forall m,n\ge N$，则 $d(x_m,x_n)<\varepsilon$。称 $X$ 是**完备**的，若 $\forall x_n\in X$ 为柯西列，则 $\exists x\in X,x_n \to x$。

**注**：实际上，收敛列一定是柯西列，即 $\{收敛列\} \subset \{柯西列\}$；而如果 $X$ 又是完备的，那么说明柯西列也一定是收敛列。因此 $X$ 完备 $\iff \{柯西列\}=\{收敛列\}$。

**命题**：$x_n$ 为柯西列，则 $\{x_n,n\ge 1\}$ 为有界集。

***例子 1***：$(\mathbb{R},d_1)$ 完备；$(\mathbb{K}^n, d_p)$ 完备；$(\ell^\infty, d_\infty)$ 完备；$(\ell^p,d_p)$ 完备；$(C[a,b],d_\infty)$ 完备。

***证明***：证明完备性的套路：

1. 任取柯西列 $x_n\in X$；
2. 找到可能的极限 $x$；
3. 证明 $x\in X$；
4. 证明 $x_n\to x$。

***例子 2***：$(C[a,b], d_p)$ **不完备**（反例如下图所示）；$\mathbb{Q}$ 不完备（因为不是闭集）；$c_{00}$ （有限个元素不为零的序列）不完备。

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/2-example.jpg)

 **定理**：度量空间 $(X,d)$

- $\forall Y\subset X$ 为完备的，则 $Y$ 为闭集；
- 若 $(X,d)$ 完备，则 $\forall Y\subset X$ 完备 $\iff Y$ 为闭集。

证明：第一条应用闭集对极限封闭的性质；第二条反向应用 $(X,d)$ 完备的性质。细节略。

**定理**：若 $x_n\in C[a,b],x_n\rightrightarrows x$（一致收敛），则 $x\in C[a,b]$。



## 4. 映射与连续性

前面从单个度量空间的角度来考虑元素的性质，现在考虑两个度量空间的对应关系，也就是映射。

对于实空间的映射 $f:(a,b)\to \mathbb{R}$，我们对连续性的定义为：$f$ 在 $t_0\in(a,b)$ 处连续，若 $\lim_{t\to t_0}f(t)=f(t_0)$。由于我们在度量空间中已经定义了距离 ，因此可以将其推广至度量空间。

假设有度量空间 $(X_1,d_1)$ 和 $(X_2,d_2)$，映射 $T:X_1 \to X_2$。

**定义**：称映射 $T$ 在 $t=t_0$ 处**连续**，若 $\forall \varepsilon > 0,\ \exists \delta > 0$，使得 $\forall t\in X_1,\ d_1(t,t_0)<\delta$，都有 $d_2(Tt, Tt_0)<\varepsilon$。若 $T$ 在 $\forall t\in X_1$ 处都连续，则称 $T$ 为**连续映射**。

***例子 1***：若 $(X_1,d_1)$ 为离散度量空间，那么任意 $T:X_1 \to X_2$ 一定是连续映射。证明只需要套用定义，取 $\delta=1/2$ 即可。

***例子 2***(Lipschitz 连续)：$\exists c>0,\ \forall s,t\in X_1$ 都有 $d_2(Ts,Tt)< c d_1(s,t)$，那么 $T$ 是连续映射。

**定理**：$T$ 为**连续映射** $\iff \forall G\subset X_2$ 为开集，那么 $T^{-1}(G)=\{x\in X_1, Tx\in G\}$ 是 $X_1$ 中的开集。

**证明**："$\Longrightarrow$"：若已知 $T$ 连续，$G$ 为开集

$G$ 为开集，就有 $\forall x_0\in T^{-1}(G),\exists \varepsilon > 0,\ B(Tx_0,\varepsilon)\subset G$

由于 $T$ 连续，则一定 $\exists \delta > 0$，使得 $\forall x\in B(x_0, \delta)$，都有 $Tx\in B(Tx_0,\varepsilon)$，因而 $B(x_0,\varepsilon)\subset X_1$

故 $T^{-1}(G)$ 为开集。

"$\Longleftarrow$"：假设 $\forall G\subset X_2$ 为开集，$T^{-1}(G)$ 在 $X_1$ 中也是开集，那么套用连续映射的定义，就能证明 $T$ 为连续映射。

证毕。

**推论**：$T$ 为**连续映射** $\iff \forall F\subset X_2$ 为闭集，那么 $T^{-1}(F)=\{x\in X_1, Tx\in F\}$ 是 $X_1$ 中的闭集。

**定理**：$T$ 在 $x_0$ 处连续 $\iff \forall x_n\in X_1, x_n\to x_0$，则 $Tx_n \to Tx_0$。

**证明**："$\Longrightarrow$"：应用定义；

"$\Longleftarrow$"：反证法，假设 $T$ 在 $x_0$ 处不连续，

那么 $\exists \varepsilon_0 > 0,\forall \delta >0, \exists x\in B(x_0, \delta)$，使得 $d(Tx_0, Tx) > \varepsilon_0$

可以取 $\delta = 1/n$，由此构造出一个序列 $x_n \in B(x_0,1/n)$，

可以知道 $x_n\to x_0$，但是却有 $d(x_n,x_0) \ge \varepsilon_0$，与假设矛盾。

证毕。

**推论**：$T:X_1\to X_2$ 处处连续 $\iff \forall x_n\to x, Tx_n \to Tx$。



## 5. Banach 不动点定理

不动点想必大家在别的地方都或多或少听说过或者用过，应该是解决很多问题的重要工具。在这一部分的内容里面则可以看到，前面讲的序列收敛性、映射在不动点定理当中的应用。

**定义**：考虑 $X\ne\varnothing, T:X\to X$，若 $x_0\in X,Tx_0=x_0$，则称 $x_0$ 为 $T$ 的**不动点**。

**定义**：$T:X\to X$，假设 $\exists 0\le \alpha < 1$，使得 $\forall x,y\in X$，都有 $d(Tx,Ty) \le \alpha d(x,y)$，则称 $T$ 为**压缩映射**。

**定理(Banach不动点定理)**：假设 $(X,d)$ 为**非空、完备**度量空间，$T:X\to X$ 为压缩映射，则 $T$ **存在唯一**的不动点。

**证明**：考虑 $x_0\in X,x_1=Tx_0,\cdots,x_n=Tx_{n-1},\cdots$，那么可以首先证明 $x_n$ 为柯西列，进而存在收敛值 $x$。 由于 $d(x_n,x_{n+1})=d(x_n,Tx_n)\to 0$，从而趋向于 $x=Tx$。之后再证明唯一性。证毕。

**定理**：假设 $(X,d)$ **非空完备**，$T:X\to X$，设 $\exists m\ge1$，$T^m$ 为压缩映射，则 $\exists! x\in X$ 使得 $Tx=x$。

**证明**：只需要证明 $S=T^m$ 的不动点都是 $T$ 的不动点，反之 $T$ 的不动点也都是 $S$ 的不动点即可。

由不动点定理可知，$S$ 存在唯一一个不动点，记为 $y_0$，即 $Sy_0=y_0$，那么 $STy_0=T^{m+1}y_0=TSy_0=Ty_0$，即 $Ty_0$ 也是 $S$ 的不动点，因此一定有 $Ty_0=y_0$，即 $y_0$ 也是 $T$ 的不动点。假设 $z_0$ 是 $T$ 的不动点，那么很容易证明他也是 $S$ 的不动点。因此 $T$ 存在唯一不动点。证毕。

***例子 1***：$c>0$，求 $\sqrt{c}$ 的数值解。可以用数值迭代，取 $f(x)=(x+\frac{c}{x})/2,D=[\sqrt{c},+\infty)$，求不动点即可。

***例子 2***：$(X=\mathbb{K}^n,d_\infty),C\in\mathbb{K}^{n\times n},b\in\mathbb{K}^n$，映射 $Tx=Cx+b$。容易证明若 $\forall i,\sum_j|a_{ij}|<1$，则 $T$ 为压缩映射。

***例子 3***：考虑 $(t_0,x_0)\in \mathbb{R}^2,a,b>0$，考虑矩形 $R=[t_0-a,t_0+a]\times[x_0-b,x_0+b]$，连续函数 $f:R\to\mathbb{R}$，假设存在 $k\ge0,|f(t,u)-f(t,v)|\le k|u-v|,\forall (t,u),(t,v)\in R$。考虑初值问题
$$
(P):\begin{cases}
x'(t)=f(t,x(t)) \\
x(t_0)=x_0
\end{cases}
$$
求上述初值问题的解 $x(t)\in C[t_0-\beta,t_0+\beta],0<\beta\le a$。

***解***：首先给出结论：如果给定 $c=\max_{(t,x)\in R}|f(t,x)|,0<\beta<\min\{a,\frac{b}{c},\frac{1}{k}\}$，那么存在唯一的 $x\in C^1[t_0-\beta,t_0+\beta]$，使得当 $t\in[t_0-\beta,t_0+\beta]$ 时，有 $x(t)\in[x_0-b,x_0+b]$ 且 $x$ 满足方程 $(P)$。下面给出证明。

由于 $x'(t)=f(t,x(t)) \Longrightarrow \int_{t_0}^t x'(\tau)d\tau=\int_{t_0}^t f(\tau,x(\tau))d\tau \Longrightarrow  x(t)=x_0+\int_{t_0}^t f(\tau,x(\tau))d\tau$，

可以证明 $|x(t)-x_0|\le c\beta <b \Longrightarrow (t,x(t))\in R$。

$(X,d_\infty)$ 完备，取 $M=\bar{B}(x_0,c\beta)$ 为闭集，因此 $M$ 为完备的，

取 $Tx=x_0+\int_{t_0}^t f(\tau,x(\tau))d\tau$，也可以证明 $d_\infty(Tx,x_0)\le c\beta \Longrightarrow Tx\in M$，即 $T:M\to M$，

又容易证明 $T$ 为压缩映射，因而存在唯一的 $x\in M$ 使得 $Tx=x$，只需要迭代即可获得 $x(t)$。

***例子 4(隐函数存在定理)***：略。



## 6. 小结

这一章当中讲解了度量空间、开集闭集、序列收敛性、柯西列、集合完备性、映射与连续性，以及最后的Banach不动点定理。现在我们已经完成了度量空间中的内容，你能够回答文章开头的问题了吗？对于度量空间这个概念有什么新的理解吗？