---
title: 泛函分析笔记2：赋范空间
date: 2020-11-10 20:15:17
tags:
  - 线性空间
  - 赋范空间
  - Banach空间
  - Hamel基
  - Schauder基
  - 线性算子
  - 算子范数
  - 线性泛函
categories:
  - Functional Analysis
mathjax: true
---

在度量空间中，我们重点关注的是两个元素之间的距离，而这一部分要引出来的赋范空间中，则对每个元素本身也赋予了“范数”，也就是“长度”。

## 1. 线性空间

线性空间，可以简单理解为对线性运算封闭的集合。那么线性运算的形式是什么呢？常见的线性运算可以写为 $T(x;a)=\sum_i a_ix_i$，因此可以看出来包含了**数乘**和**加法**两种基本运算，因此线性空间必然需要对数乘和加法封闭，除此之外，为了保证运算体系的自洽性，我们还需要规定一些其他的运算规则。最后，给出来线性空间的定义：

<!--more-->

考虑 $\mathbb{K=C\ or\ R}, X\ne \varnothing,\forall x,y\in X,\forall \lambda\in\mathbb{K}$，可以定义 $x+y\in X,\lambda x\in X$，如果满足

1. $x+y=y+x$
2. $(x+y)+z=x+(y+z)$
3. $\exists 0\in X,\forall x\in X, x+0=0+x=x$
4. $\forall x,\exists! y\in X,x+y=0,y=-x$
5. $\alpha(\beta x)=(\alpha\beta)x$
6. $1x=x$
7. $(\alpha+\beta)x=\alpha x+\beta x$
8. $\alpha(x+y)=\alpha x+\alpha y$

则称 $X$ 为 $\mathbb{K}$ 上的**线性空间**。

***例子 1***：$S=\{(x_n)_{n\ge1},x_n\in\mathbb{K} \}$，定义加法 $(x_n)_{n\ge1}+(y_n)_{n\ge1}=(x_n+y_n)_{n\ge1}\in S$，定义数乘 $\lambda(x_n)_{n\ge1}=(\lambda x_n)_{n\ge1}\in S$，那么可以验证 $S$ 为线性空间。

***例子 2***：$C[a,b]$，定义 $(f+g)(t)=f(t)+g(t), (\lambda f)(t)=\lambda f(t)$，可以验证 $C[a,b]$ 为线性空间。

**定义**：$X$ 为 $\mathbb{K}$ 上的线性空间，$Y\subset X$，称 $Y$ 为 $X$ 的线性子空间，若 $\forall x,y\in Y,\forall \lambda\in\mathbb{K}$，有 $x+y\in Y,\lambda x\in Y$，并且 $Y$ 本身也是线性空间。

***例子 3***：$\ell^\infty \subset S$ 是线性空间。

***例子 4***：$\ell^p \subset S,1\le p < \infty$ 是线性空间。

***例子 5***：$\ell^p \subset c_0(x_n\to0的序列\{(x_n)\}) \subset c(x_n收敛的序列) \subset \ell^\infty\subset S(无穷维序列)$，每一个都是线性空间。

***例子 6***：$P$ 所有多项式的集合，$P\subset C[a,b]$ 也是线性空间。

**命题**：若 $X$ 为线性空间，$(Y_i)_{i\in I}$ 为 $X$ 的一族线性子空间，则 $Y=\bigcap_{i\in I}Y_i$ 仍然是 $X$ 的线性子空间。

**定义**：$X$ 为线性空间，$M\subset X$ 非空，令 $\mathcal{E}$ 为所有包含 $M$ 的线性子空间，那么一定有 $X\in\mathcal{E}$，因此 $\mathcal{E}$ 一定非空。令 $Z=\bigcap_{Y\in\mathcal{E}}Y$ 也一定是 $X$ 的线性子空间，记 $Z=\text{span}(M)$ 称为 $M$ **生成的线性子空间**。

**命题**：若 $M\subset X$ 非空，那么 $Z=\text{span}(M) = \{\sum_i^n \lambda_i x_i,n\ge1,x_i\in M,\lambda_i\in\mathbb{K}\}$ 为包含 $M$ 的 $X$ 的**最小线性子空间**。

证明：略。

***例子 1***：$X=S,e_n=(0,\cdots,0,1,0,\cdots)\in S, M=\{e_n,n\ge1\}\subset S$，则 $\text{span}(M)=c_{00}$(只有有限个元素非零的无穷维序列)。

**定义**：$M\subset X$ 为**线性无关**的，若 $\forall x_1,...,x_n\in M$ 两两不等，都有 $x_1,...,x_n$ 都线性无关(也即任意有限个元素都线性无关)。

***例子 2***：$X=C[a,b],\alpha_1<\alpha_2<\cdots<\alpha_n\le\cdots, f_n=e^{\alpha_n t}\in C[a,b]$，那么取 $M=\{f_1,f_2,\cdots\}$ 是线性无关的 $\iff \forall n\ge1, f_1,...,f_n$ 线性无关。

*证明*：利用线性相关的定义，重复求导、相减的过程，细节略。

**定义**： $X$ 为线性空间，若 $X$ 中存在 $n$ 个元素线性无关，但任意 $n+1$ 个元素都线性相关，则称 $X$ 为 $n$ **维**的，记为 $\text{dim}X=n$。若 $\forall n\ge1,\exists x_!,...,x_n$ 线性无关，则称 $X$ 为**无穷维**的。

**命题**：$\text{dim}X=n,\forall x\in X$ 都存在唯一的系数 $a_1,...,a_n$ 使得 $x=a_1x_1+\cdots+a_nx_n$。

证明：略。

对于复空间 $\mathbb{C}^n$，认为 $\text{dim}\mathbb{C}^n=2n$。

**定义**：$X$ 为线性空间，$M\subset X$ 线性无关，若 $\text{span}(M)=X$，则称 $M$ 为 $X$ 的 **Hamel 基**。

> **命题**：$\forall x\in X$，则 $\exists x_1,...,x_n\in M$ 两两不等，$\exists \lambda_1,...,\lambda_n\in\mathbb{K}$ 均不为 0，使得 $x=\lambda_1 x_1+...+\lambda_n x_n$，并且上述**表示唯一**。
>
> 证明：略。

***例子 1***：$\{e_1,e_1,...\}$ 为 $c_{00}$ 的 Hamel 基。

**定理**：$X$ 为线性空间，$M\subset X$ 线性无关，则 $\exists N$ 为 $X$ 的 Hamel 基，使得 $M\subset N$。

> **小结**：这一部分主要是讲解了线性空间的定义，最关键的概念就在于
>
> 1. 对于加法和数乘封闭；
> 2. 可以找到一组基，任意向量都可以用基的线性组合来表示，并且表示方法唯一。



## 2. 赋范空间与Banach空间

假设 $X$ 为 $\mathbb{K}$ 上的线性空间，定义范数运算 $\Vert\cdot\Vert:X\to \mathbb{R}$，满足如下条件：

1. $\Vert x\Vert \ge 0$
2. $\Vert x\Vert =0\iff x=0$
3. $\Vert\lambda x\Vert=|\lambda|\cdot\Vert x\Vert$
4. $\forall x,y\in X, \Vert x+y\Vert\le \Vert x\Vert + \Vert y\Vert$

则称 $\Vert\cdot\Vert$ 为 $X$ 上的范数，$(X,\Vert\cdot\Vert)$ 为**赋范空间**。

**定义**：在赋范空间中，$\forall x,y\in X$，若定义 $d(x,y)=\Vert x-y\Vert$，那么该方法定义的 $d(x,y)$ 也是度量，称为 $\Vert\cdot\Vert$ **诱导的度量**。若此时 $(X,d)$ 为**完备空间**，则称 $(X,\Vert\cdot\Vert)$ 为 **Banach 空间**（即完备的赋范空间）。

**注**：由于范数的定义中第 3 条存在，以及诱导度量只有 $x-y$ 决定，使得诱导度量相比于一般定义的度量还具有一些特别性质，例如：

- 平移不变性 $d(x+a,y+a)=d(x+y)$
- 齐次性 $d(\lambda x,\lambda y)=|\lambda| d(x,y)$

***例子 1***：$(\mathbb{K}^n,\Vert\cdot\Vert_\infty),(\mathbb{K}^n,\Vert\cdot\Vert_p),(\ell^\infty,\Vert\cdot\Vert_\infty),(\ell^p,\Vert\cdot\Vert_p),(c_{0},\Vert\cdot\Vert_\infty),(c,\Vert\cdot\Vert_\infty)$ 均为 Banach 空间。

***例子 2***：$S=\{(x_n)_{n\ge1},x_n\in\mathbb{K}\},d(x,y)=\sum_n \frac{1}{2^n}\frac{|x_n-y_n|}{1+|x_n-y_n|}$，**不存在** $S$ 上的范数 $\Vert\cdot\Vert$ 使 $d$ 被诱导出，因为 $d$ 不满足齐次性。

 ***例子 3***：$(C[a,b],\Vert\cdot\Vert_\infty)$ 为 Banach 空间，$(C[a,b],\Vert\cdot\Vert_p)$ 不是 Banach 空间。

***例子 4***：离散度量不满足齐次性，因此不可能由范数诱导出。

**命题 1**：$(X,\Vert\cdot\Vert_\infty)$ 为赋范空间，$Y$ 为 $X$ 的线性子空间。若 $Y$ 为 Banach 空间，则 $Y$ 在 $X$ 中必为闭集。

**命题 2**：$(X,\Vert\cdot\Vert_\infty)$ 为 Banach 空间，$Y\subset X$ 为闭集线性子空间，则 $Y$ 必为 Banach 空间。

证明：赋范空间的线性子空间仍然是赋范空间，完备空间一定要是闭集。细节略。

> 只需要掌握关键点，那么上面的命题都可以由下面的关键点轻松导出：
>
> 1. 完备空间一定是闭集；完备空间的子空间也完备 $\iff$ 该子空间为闭集；
> 2. 赋范空间 + 完备性 = Banach 空间。

**命题**：若 $(X,\Vert\cdot\Vert)$ 为 Banach 空间，$x_n\in X$，且 $\sum_n^\infty \Vert x_n\Vert < \infty$，则 $\sum^\infty x_n$ 收敛。

证明：完备空间中，证明序列收敛可以证明序列是柯西列。

**定义**：$(X,\Vert\cdot\Vert)$ 为赋范空间，$e_n\in X(n\ge 1)$，称 $(e_n)_{n\ge1}$ 为 $X$ 的一组 **Schauder 基**，若 $\forall x\in X, \exists ! \lambda_n,x=\sum^\infty_n \lambda_n e_n$。

**命题**：若 $X$ 有 Schauder 基，那么 $X$ 一定是可分的。

证明：可以取 $M=\{\sum^n_{i=1}x_ie_i \in X, n\ge1, x_i\in\mathbb{Q} \}$，是可数个可数集的并。

> Hamel 基与 Schauder 基的区别：
>
> 1. 最主要的区别是 Hamel 基可以是有限维也可以是无穷维的，这由集合 $X$ 的维数决定，而 Schauder 基则只定义在无穷维上；
> 2. 前者针对线性空间，后者针对 Banach 空间。



## 3. 有限维赋范空间

有限维赋范空间相比于一般的赋范空间有很多很好的性质，比如所有范数等价、有限维赋范空间都是 Banach 空间。

假设 $X$ 为 $\mathbb{K}$ 上的线性空间，并且存在两个范数 $\Vert\cdot\Vert_1,\Vert\cdot\Vert_2$，称二者等价，若
$$
\exists \alpha,\beta > 0,\forall x\in X,\quad \alpha\Vert x\Vert_1\le \Vert x\Vert_2\le \beta\Vert x\Vert_2
$$
此时 $(X,\Vert\cdot\Vert_1),(X,\Vert\cdot\Vert_2)$ 有：

- 相同的收敛列、柯西列；
- 相同的开集、闭集、闭包、内部；
- $(X,\Vert\cdot\Vert_1)$ Banach $\iff (X,\Vert\cdot\Vert_2)$ Banach；

> **引理**：$(X,\Vert\cdot\Vert)$，$Y\subset X$ 为有限维线性子空间，设 $e_1,...,e_n$ 为 $Y$ 的一组基，则 $\exists c > 0,\forall \lambda_1,...,\lambda_n \in \mathbb{K}$，有
> $$
> c(|\lambda_1|+\cdots+|\lambda_n|) \le \Vert\lambda_1 e_1+\cdots+\lambda_n e_n\Vert \le \max_i\Vert e_i\Vert (|\lambda_1|+\cdots+|\lambda_n|)
> $$
> 证明：需要用到 Bolzano-Weierstrass 定理，即**有界列存在收敛子列**。反证法，证明略。
>
> **定理**：若 $\text{dim}X<\infty$，则 $X$ 上的**范数互相等价**，且均为 Banach 空间。
>
> 证明：只需要证明所有范数与 $\Vert\cdot\Vert_1$ 范数等价，且 $(X,\Vert\cdot\Vert_1)$ 构成 Banach 空间。

**推论**：$(X,\Vert\cdot\Vert)$，若 $Y\subset X$ 为有限维线性子空间，则 $Y$ 为闭集。

有限维赋范空间除了这个良好的性质（一定是 Bananch 空间）之外，还有其他的好性质。下面引入紧集的概念。

**定义**：$(X,d)$，$M\subset X$ 为紧集，若 $\forall x_n\in M,\exists n_k\uparrow \infty,\exists x\in M, x_{n_k}\to x(k\to \infty).$

实际上，紧集的概念跟完备性的概念有点像，前者是任意序列一定存在收敛子列，后者说明柯西列一定是收敛列。他们都跟序列的收敛性有关，后面也可以看到，他们都跟闭集有一定的关系。

实际上，紧集一定是有界闭集，但是闭集不一定是紧集。不过对于有限维赋范空间来说，二者等价，后面会有定理专门证明。

**定理**：$(X,d)$，若 $M\subset X$ 为**紧集**，则 $M$ 一定**完备**，并且一定是**有界闭集**。

**定理**：$(X,d)$，$M\subset X$ 为紧集，$N\subset M$，则 $N$ 为紧集 $\iff N$ 为闭集。

证明：略。

**定理**：$(X,\Vert\cdot\Vert)$，$\text{dim}X=n<\infty$，$M\subset X$ 为紧集 $\iff M$ 为有界闭集。

**证明**：必要性易证，充分性证明的关键是首先找到 Hamel 基表示，将在 $M$ 中寻找收敛子列的任务分解到对每一个坐标维度上寻找收敛子列，而这可以根据 Bolzano-Weiertrass 定理得到，然后再根据每个坐标为度上的收敛子列得到 $M$ 中的收敛子列。证毕。

**引理**：$(X,\Vert\cdot\Vert)$，$M\subsetneq X$ 为闭集线性子空间，对于 $\forall 0 < \theta < 1$，$\exists z\in X,\Vert z\Vert =1,\forall y\in M,\Vert z-y\Vert \ge \theta.$

**证明**：固定 $\forall v\in Y^c$，令 $a=\inf_{y\in Y}\Vert v-y\Vert$，那么 $a>0$，否则的话与 $Y$ 是闭集相矛盾。因此存在 $y_0\in Y$ 使得 $a\le \Vert v-y_0\Vert \le a/\varepsilon$。那么我们就可以把这个 $v$ 平移到 $0$ 点附近，同时也把对应的 $y_0$ 平移到原点附近，也就是取 $y=\frac{v-y_0}{\Vert v-y_0\Vert}$，那么 $\Vert y\Vert=1$，并且可以验证 $\forall z\in X$，都有 $\Vert y-z\Vert \ge \varepsilon.$ 证毕。

**定理**(F.Riesz)：$(X,\Vert\cdot\Vert)$，则 $\text{dim}X<\infty \iff \bar{B}(0,1)$ 为紧集。

**证明**：必要性易证。充分性可以利用反证法，若维度无穷，则可以构造出特殊的序列，使得序列中任意两个元素的距离都大于 $1/2$（这要用到上一个引理），从而不存在收敛子列，即 $\bar{B}(0,1)$ 不是紧集。证毕。

**推论**：$(X,\Vert\cdot\Vert)$，则 $\text{dim}X<\infty\iff S(0,1)=\{x:\Vert x\Vert=1\}$ 为紧集。

**定义**：$(X,d)$，$M\subset X$ 非空，$x_0\in X$，令
$$
\rho(x_0, M) = \inf_{y\in M} d(x_0, y)
$$
称为 $x_0$ 到 $M$ 的距离。若 $\exists y_0\in M$，使得 $\rho(x_0,M)=d(x_0,y_0)$，则称 $y_0$ 为 $x_0$ 在 $M$ 中的最佳逼近元。

**命题**：设 $M$ 为非空紧集，则 $\forall x_0 \in X$，$x_0$ 在 $M$ 中的最佳逼近元存在。

**命题**：$(X,d), M$ 为 $X$ 的有限维线性子空间，$\forall x_0\in X, \rho(x_0,M)=\inf_{y\in M}\{\Vert x_0-y\Vert\}$，则 $x_0$ 在 $M$ 中的最佳逼近元一定存在。

证明：略。

> **小结**：本部分主要证明了有限维赋范空间的良好性质：
>
> 1. 有限维赋范空间**所有范数等价**，且均为 **Banach 空间**；
> 2. 有限维赋范空间**紧集**（即任意序列存在收敛子列） $\iff$ **有界闭集**；

## 4. 有界线性算子

### 4.1 线性算子

**定义**：$X,Y$ 为 $\mathbb{K}$ 上的线性子空间，$D(T)\subset X$ 为线性子空间，$T:D(T)\to Y$ 为**线性算子**，若 $\forall x,y\in D(T),\forall \lambda \in \mathbb{K}$，都有
$$
T(x+y)=Tx+Ty,\quad T(\lambda x)=\lambda Tx \\
\iff T(\lambda x+\mu y) = \lambda Tx + \mu Ty.
$$
**定义**：零空间 $N(T)=\text{ker}(T)=\{x\in D(T), Tx=0\}=T^{-1}(0),$ 像空间 $R(T)=\{Tx, x\in D(T)\}.$

**命题**：$N(T)$ 为 $D(T)$ 的线性子空间，$R(T)$ 为 $Y$ 的线性子空间。

**命题**：$T$ 为单射 $N(T)=\{0\}$。

**命题**：$M\subset D(T)$ 为 $n$ 维线性子空间，则 $\text{dim} T(M) \le n$。

证明：略。

### 4.2 算子范数

**定义**：对于线性算子，若 $\exists c\ge 0,\forall x\in D(T), \Vert Tx\Vert_Y \le c\Vert x\Vert_X$，则称 $T$ 是**有界的**。使该式成立的最小 $c$ 称为 $T$ 的**范数**，等价于
$$
\Vert T\Vert = \sup_{x\in D(T),x\ne 0} \frac{\Vert Tx\Vert}{\Vert x\Vert} = \sup_{x\in D(T),\Vert x\Vert \le 1} \frac{\Vert Tx\Vert}{\Vert x\Vert} = \sup_{x\in D(T),\Vert x\Vert < 1} \frac{\Vert Tx\Vert}{\Vert x\Vert}
$$
***例子 1***：$X=Y=C[0,1],(Tx)(t)=\int_0^t x(\tau)d\tau$，则 $\Vert T\Vert = 1, x(t)\equiv1$ 时取到。

***例子 2***：$X=C[-1,1],f(x)=\int_{-1}^0x(t)dt - \in_0^1x(t)dt,|f(x)|\le 2\Vert x\Vert_\infty$，但是 $2$ 取不到。

***例子 3***：$T:C^1[0,1]\to C[0,1],T(x)=x'$，那么 $T$ 为线性**无界**算子。令 $x_n(t)=t^n,t\in[0,1]$，则 $(Tx_n)(t)=nt^{n-1}.$

***例子 4***：$A=(a_{ij})_{n\times n},a_{ij}\in\mathbb{K},Tx=Ax.\Vert T\Vert=?$

**定理**：$X,Y$ 为 $\mathbb{K}$ 上的赋范空间，假设 $\text{dim}X=n<\infty$，$T:X\to Y$ 是线性算子，那么 $T$ 一定是有界的。

证明：用基来表示 $X$ 中的元素，并利用性质 $\Vert\lambda_1 e_1+\cdots+\lambda_n e_n\Vert \ge c(|\lambda_1|+\cdots+|\lambda_n|)$ 即可得证。证毕。

**定理**：$X,Y$ 为 $\mathbb{K}$ 上的赋范空间，$T:X\to Y$ 是线性算子，那么以下命题等价

1. $T$ 有界
2. $T$ 处处连续
3. $T$ 在 $X$ 上某一点连续

**证明**：$(1)\to(2),(2)\to(3)$ 易证；

$(2)\to(1)$：由于 $T$ 连续，在 $x=0$ 点附近，存在 $\delta>0, \forall x \in B(0,\delta)$，有 $\Vert Tx\Vert\le1$，因而 $\Vert Tx\Vert \le \frac{2}{\delta}\Vert x\Vert$；

$(3)\to(2)$：由于 $T$ 为线性算子，那么只要在任一 $x_0$ 点处连续，经过平移可以得到 $T$ 在任意一点处都连续。

证毕。

**推论**：$X,Y$ 为 $\mathbb{K}$ 上的赋范空间，$T:X\to Y$ 线性有界，那么 $N(T)$ 为 $X$ 的闭集线性子空间。

用符号 $B(X,Y)$ 来表示 $X\to Y$ 的有界线性算子。

**定理**：假设 $X$ 为赋范空间，$Y$ 为 Banach 空间，那么 $B(X,Y)$ 为 Banach 空间。

证明：略。

> **小结**：本部分定义了线性算子，以及有界线性算子的范数。由于**有界线性算子等价于连续算子**，今后主要研究的也是有界线性算子。

## 5. 有界线性泛函

从赋范空间 $X$ 到 $\mathbb{K}$ 的线性算子称为 $X$ 上的**线性泛函**。

**定义**：$(X,\Vert\cdot\Vert)$，用 $X'$ 表示 $X$ 上所有有界线性泛函全体，称之为 $X$ 的**拓扑对偶空间**。

若 $X$ 上的一组基为 $\{e_1,e_2,\cdots\}$（有限维或者无限维均可），那么 $f:X\to\mathbb{K}$ 由 $f(e_1),f(e_2),\cdots$ 唯一确定。

**定义**：$(X,Y)$ 赋范空间，若存在线性算子 $T:X\to Y$，并且 $\Vert Tx\Vert_Y = \Vert x\Vert_X,\forall x\in X$，则称 $X,Y$ 为**等距同构**的，因此可以视 $X,Y$ 为同一空间。

**命题**：$(\mathbb{K}^n,\Vert\cdot\Vert_2)' = (\mathbb{K}^n,\Vert\cdot\Vert_2)$（此命题的含义为 $(\mathbb{K}^n,\Vert\cdot\Vert_2)$ 的对偶空间等价于 $n$ 维空间，也就是说每一个有界线性泛函都可以映射为 $\mathbb{K}^n$ 上的一个点/向量）

证明：略。

对于形如 $(X,\Vert\cdot\Vert_1)' = (Y,\Vert\cdot\Vert_2)$ 的命题，证明思路如下。

**证明**：要想证明两个空间等价，那么只需要找到一个线性映射  $T:(X,\Vert\cdot\Vert_1)' \to (Y,\Vert\cdot\Vert_2)$，使得映射前后在两个空间的范数相等。考虑对 $\forall f\in (X,\Vert\cdot\Vert_1)'$，取 $f\mapsto (f(e_1),f(e_2),\cdots,f(e_n))$，那么按照以下步骤证明：

1. 证明对于任意 $\forall f\in (X,\Vert\cdot\Vert_1)'$，的确有 $(f(e_1),f(e_2),\cdots,f(e_n))\in Y$，即映射的源空间和像空间的确是 $X,Y$；
2. 证明 $T$ 为单射（即证明若 $Tf=Tg$，那么有 $f=g$）；
3. 证明 $T$ 为满射，此时 $T$ 为双射（即证明 $\forall \alpha \in Y$，存在 $f \in (X,\Vert\cdot\Vert_1)'$ 使得 $Tf=\alpha$，一般需要构造线性算子 $f$）；
4. 证明 $\Vert Tf\Vert = \Vert f\Vert$。

证毕。

因此按照上面的方法，还可以证明如下几个命题。

- $(\mathbb{K}^n,\Vert\cdot\Vert_p)' = (\mathbb{K}^n,\Vert\cdot\Vert_q),\frac{1}{p}+\frac{1}{q}=1$
- $(c_0, \Vert \cdot\Vert_\infty)'=(\ell^1,\Vert \cdot\Vert_1)$
- $(\ell^1,\Vert \cdot\Vert_1)'=(\ell^\infty,\Vert \cdot\Vert_\infty)$
- $(\ell^p,\Vert \cdot\Vert_p)'=(\ell^q,\Vert \cdot\Vert_q)$

> **小结**：有界线性泛函可以映射到某个我们熟悉的空间，对于后面的处理很有用。