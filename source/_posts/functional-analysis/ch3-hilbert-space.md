---
title: 泛函分析笔记3：内积空间
date: 2020-12-11 08:18:56
tags:
  - 内积空间
  - Hilbert空间
  - 共轭双线性泛函
  - 伴随算子
  - Riesz表示定理
  - 直和
  - 正交补
  - 标准正交基
  - 标准正交集
  - Bessel不等式
categories:
  - Functional Analysis
mathjax: true
---

我们前面讲了距离空间、赋范空间，距离空间赋予了两个点之间的距离度量，范数赋予了每个点自身的长度度量，而范数则可以导出距离。本章要讲的内积可以看成是更加统一的定义，因为从内积我们可以导出范数，进而导出距离。因此内积空间是一个“更小”的空间，在此基础上结合完备性我们引出了 Hilbert 空间，后续我们的研究也大多集中在 Hilbert 空间上。

## 1. 内积空间

**定义**：$X$ 为 $\mathbb{K}$ 上的线性空间，$\langle \cdot,\cdot\rangle :X\times X\to \mathbb{K}$，若满足如下条件

1. $\langle \lambda x+\mu y, z\rangle  = \lambda\langle x,z\rangle +\mu\langle y,z\rangle$
2. $\overline{\langle x,y\rangle }=\langle y,z\rangle$
3. $\forall x\in X, \langle x,x\rangle  \ge 0$
4. $\langle x,x\rangle =0 \iff x=0$

则称 $(X,\langle \cdot,\cdot\rangle )$ 为**内积空间**。可以用内积定义范数 $\Vert x\Vert = \langle x,x\rangle ^{1/2}$。若得到的 $(X,\Vert\cdot\Vert)$ 为 Banach 空间，那么 $(X,\langle \cdot,\cdot\rangle )$ 为 **Hilbert 空间**。

<!--more-->

**定理**：$(X,\langle \cdot,\cdot\rangle ),\forall x,y\in X$ 有

- $|\langle x,y\rangle |\le \Vert x\Vert \cdot\Vert y\Vert$（Schwartz 不等式）等号成立 $\iff x,y$ 线性相关 $\iff y=0$ 或 $x=cy$
- $\Vert x+y\Vert\le\Vert x\Vert+\Vert y\Vert$ 等号成立 $\iff x,y$ 线性相关 $\iff y=0$ 或 $x=cy$

**定理**：$(X,\langle \cdot,\cdot\rangle )$，设 $x_n\to x,y_n\to y$，则 $\langle x_n,y_n\rangle  \to \langle x,y\rangle $（此定理说明**内积为连续映射**）。

证明：略。

**命题**：若 $\Vert\cdot\Vert$ 为 $X$ 上的范数，若 $\forall x,y\in X$ 都满足平行四边形等式 $\Vert x+y\Vert^2 + \Vert x-y\Vert^2=2(\Vert x\Vert^2 + \Vert y\Vert^2)$，则存在 $X$ 上的内积 $\langle \cdot,\cdot\rangle $，使得 $\Vert x\Vert=\langle \cdot,\cdot\rangle ^{1/2}.$

证明：较复杂，略。

*例子 1*：$X=\mathbb{K}^2,\Vert(x,y)\Vert_\infty$ 不是内积空间，反例比如 $x=(1,1),y=(1,-1)$，验证平行四边形等式不成立即可。

*例子 2*：$(\ell^\infty,\Vert\cdot\Vert_\infty)$ 不是内积空间，反例如 $x=(1,0,0,...),y=(0,-1,0,...)$。

实际上对于空间 $X=\mathbb{K}^n,n>0$，$\Vert x\Vert_p$ **只有在 $p=2$ 的时候才是内积空间**，其余情况均不是内积空间。

对于空间 $(\ell^p,\Vert\cdot\Vert_p)$ 也**只有在 $p=2$ 的时候才是内积空间**。

*例子 3*：$C[0,1],\Vert x\Vert_\infty$ 不是内积空间，反例比如 $x(t)=1+t,y(t)=1-t$，验证平行四边形等式不成立即可(说明找不到合适的内积定义来导出无穷范数)。

类比上面的例子，我们也可以对连续函数定义 $2$ 范数($p$ 范数)。首先定义内积
$$
\begin{aligned}
\langle x,y\rangle =\int_a^b x(t)\overline{y(t)} dt \quad\Rightarrow\quad \Vert x\Vert=\langle x,x\rangle ^{1/2} \\
\Vert x\Vert_p = \left(\int_a^b |x(t)|^p dt\right)^{1/p}
\end{aligned}
$$
同样地，**只有在 $p=2$ 的时候 $(C[a,b],\Vert x\Vert_p)$ 才是内积空间。**

到这里大家基本了解了内积空间的特点，他比一般的赋范空间更严格，度量空间就更不用说了。根据初中的知识，有了内积我们就能计算夹角了，不过这里我们不讲夹角，而是考虑**正交**和**正交补**的概念。

> **小结**：这一部分讲了内积运算的定义，并且由内积可以导出范数的定义，但是内积比范数的要求更严格，因此对于某个范数，可以通过验证平行四边形等式来验证其是否可以由内积运算来导出。



## 2. 正交补与正交投影

内积空间 $(X,\langle \rangle )$ 中，称 $x,y$ **正交**，若 $\langle x,y\rangle =0$，记为 $x\perp y$。$M\subset X$ 非空，定义其**正交补** $M^{\perp}=\{x\in X:x\perp y,\forall y\in M\}.$

**命题**：$M^{\perp}$ 为 $X$ 的**闭集线性子空间**。

证明：易证 $M^{\perp}$ 为线性子空间，然后再用内积的是连续映射证明 $M^{\perp}$ 为闭集。

这里插入一个**最佳逼近元**的概念。定义 $\xi(x_0,M)=\inf_{y\in M}d(x_0,y)$，若存在 $y_0\in M$ 使得 $d(x_0,y_0)=\xi(x_0,M)$，那么就称 $y_0$ 为最佳逼近元。什么情况下最佳逼近元存在呢？

1. 当 $M$ 为非空紧集的时候，$y_0$ 存在（因为紧集一定是有界闭集）。
2. 若 $X$ 为赋范空间，$M$ 为 $X$ 的有限维线性子空间，则 $y_0$ 存在（因为有限维赋范空间都完备，$M$ 为闭集）。

**定理**：$(X,\langle \rangle )$，$M$ 为 $X$ 非空凸子集，且 $M$ 完备，则 $\forall x_0\in X$，$\exists!y_0\in M$ 为最佳逼近元，并且有 $x_0-y_0\in M^{\perp}$。

证明：先找到 $y_n\in M,d(x_0,y_n)\le\xi(x_0,M)+\frac{1}{n}$，证明其为柯西列，由于 $M$ 完备，得到最佳逼近元的存在性。再由 $M$ 的凸性质证明唯一性。证毕。

对于线性空间 $X$，$M,N$ 为 $X$ 的线性子空间，称 $X$ 为 $M,N$ 的**直和**，若 $\forall x\in X,\exists! m\in M,n\in N,x=m+n$，记为 $X=M\oplus N.$

很容易验证 $X=M\oplus N \iff X=\text{span}(M\cup N), M\cap N=\{0\}.$

**定理**：设 $H$ 为 Hilbert 空间，$M$ 为 $H$ 的闭线性子空间，则 $H=M\oplus M^{\perp}.$

证明：$M$ 完备，对 $\forall x\in H$，$\exists!y\in M$，使得 $\Vert x-y\Vert=\xi(x,M)$，并且 $x-y\in M^{\perp}$。证毕。

**定理**：设 $H$ 为 Hilbert 空间，$M$ 为 $H$ 的闭线性子空间，则 $(M^{\perp})^{\perp}=M.$

证明：略。

上面两条定理当中，只有 $M$ 是 $H$ 的闭线性子空间才有如此良好的性质！因为只有闭线性子空间才能认为 $M$ 是尽可能“丰满”的。举个例子，$\mathbb{R}^3$ 中，令 $M=\{e_1=(1,0,0)\}$，那么 $M^{\perp}$ 为 $y-z$ 平面，$(M^{\perp})^{\perp}$ 为 $x$ 轴，相比于原始的 $M$ 扩展到无穷远处了。

**推论**：设 $H$ 为 Hilbert 空间，$M\subset H$ 非空，则 $\overline{\text{span}M}=H \iff M^{\perp}=\{0\}.$

**引理**：$M^{\perp}=(\bar{M})^\perp,\quad (\text{span}M)^{\perp}=M^{\perp}.$

证明：略。

**NOTE**：这个引理实际上说明了正交补空间在定义的时候已经是“最大化的”，即使原空间 $M$ 稍微扩张一点点，其正交补空间仍然保持不变。

设 $H$ 为 Hilbert 空间，$M$ 为 $H$ 的闭线性子空间，那么对于 $\forall x\in H$，存在唯一的分解 $x=y+z,y\in M,z\in M^\perp.$ 记 $P_Mx=y$，称 $P_M$ 为从 $H$ 到 $M$ 上的**正交投影**，其有如下性质：

1. $P_M$ 为线性算子；
2. $P_M$ 有界，并且 $P_M=1, M\ne\{0\}$，$P_M=0,M=\{0\}$；
3. $P_M^2=P_M$；
4. $R(P_M)=M, N(P_M)=M^{\perp}$。

> **小结**：本小节给出了正交补的定义
>
> 1. 不论原空间 $M$ 如何，正交补空间 $M^{\perp}$ 总是有一些良好的性质：1）闭线性子空间；2）“最大化的”；
> 2. 如果原空间 $M$ 同时有良好的性质（闭线性子空间），那么他们两个就是对整个空间 $H$ 的良好分割。



## 3. 标准正交基

内积空间 $(X,\langle \rangle )$ 中，$M\subset X$ 为**正交集**，若 $\forall x,y\in M,x\ne y,\Rightarrow x\perp y.$ 进一步若 $M$ 中任意元素 $\Vert x\Vert=1$，则称 $M$ 为**标准正交集**。

对于一个标准正交序列 $M=\{e_n,n\ge1\}$，有 **Bessel 不等式**
$$
\sum_{i=1}^\infty |\langle x,e_i\rangle |^2 \le \Vert x\Vert^2.
$$

有了标准正交集的概念，我们很容易联想到正交基。下面列出的正交集的性质都可以类比基，但是随便取一个标准正交集，其并不一定完备，因此二者又有细微的差别。

**定理**：$H$ 为 Hilbert 空间，$\{e_1,e_2,...\}$ 为 $H$ 的标准正交集，那么有

- $\forall \lambda_n\in\mathbb{K}, \sum^\infty_n \lambda_ne_n$ 收敛 $\iff \sum_n^\infty |\lambda_n|^2<\infty$
- 若 $x=\sum^\infty_n \lambda_n e_n$，则 $\lambda_n=\langle x,e_n\rangle $
- $\forall x\in H, \sum^\infty_n\langle x,e_n\rangle e_n$ 收敛

证明：由于涉及到无穷级数，因此证明过程中需要首先考虑 $S_N=\sum_{n=1}^N \lambda_n e_n$ 的情况，再证明 $N\to\infty$ 的时候极限成立。细节略。

如果对于标准正交集 $M$ 我们有 $\overline{\text{span}M}=H$，那么称 $M$ 在 $H$ 中为**完全的**。实际上这里很容易联想到完备正交基，我们知道空间中任意一点都可以用基的线性组合来表示，如果 $M$ 是完全的，那么我们可以有相似的结论。

**命题**：首先定义 $\forall x\in X, M_x=\{e\in M, \langle x,e\rangle \ne0\}$，那么一定有 $M_x$ 是至多可数的。

证明：定义 $M_{x,n}=\{e\in M, |\langle x,e\rangle |\ge \frac{1}{n}\}$，$M_x=\cup^\infty_{n=1} M_{x,n}$，只需证明 $\forall n\ge1, M_{x,n}$ 为至多可数集。

假设 $M_{x,n}$ 中两两不等的元素可以表示为 $e_1,...,e_N$，那么 $\frac{N}{n^2}\le\sum_{n=1}^N|\langle x,e_i\rangle |^2\le\Vert x\Vert^2 \Rightarrow N\le n^2\Vert x\Vert^2$，说明 $M_{x,n}$ 中的元素个数是有限的。证毕。

**定理**：$H$ 为 Hilbert 空间，$M$ 为 $H$ 的标准正交集，则下述命题等价：

1. $M$ 为完全的；
2. $\forall x\in H,x=\sum_{e\in M}\langle x,e\rangle e$；
3. $\forall x\in H, \Vert x\Vert^2=\sum_{e\in M}|\langle x,e\rangle |^2.$
4. $\forall x,y\in H,\langle x,y\rangle =\sum_{e\in M}\langle x,e\rangle \langle e,y\rangle $

证明：首先用反证法证明 $2\Rightarrow1,3\Rightarrow1.$ 然后考虑 $\forall x\in H, M_x=\{e_1,e_2,...\}$，令 $y=\sum^\infty_{i=1} \langle x,e_i\rangle e_i$， $z=y-x$，容易证明 $\langle z,e_i\rangle =0,\forall i\ge1$，那么就有 $z\in M^{\perp}=\{0\}$，从而 $x=y=\sum^\infty_{i=1} \langle x,e_i\rangle e_i$，$1\Rightarrow2$。其余证明可以参考课本。此处省略。

对于内积空间 $X$ 的一列线性无关元素 $\{x_1,...,x_n\}$，那么存在一组标准正交序列 $e_1,...,e_n$ 使得
$$
\text{span}\{x_1,...,x_n\} = \text{span}\{e_1,...,e_n\}
$$
其中一种获取方法为 **Gram-Schmidt 标准正交化方法**，即

1. 首先取 $e_1=\frac{x_1}{\Vert x_1\Vert}$
2. 然后 $v_2=x_2-\langle x_2,e_1\rangle e_1, e_2 = \frac{v_1}{\Vert v_2\Vert}$
3. 上述过程重复进行。

**定理**：$H$ 为 Hilbert 空间，$M$ 为 $H$ 的标准正交集，则存在 $N$ 为完全标准正交集，$M\subset N.$

**推论**：任意 Hilbert 空间均有完全标准正交集。

证明：可以根据有限维空间、无限维可分空间进行讨论，应用 Gram-Schmidt 标准正交化方法即可获得完全标准正交集。

实际上如果我们能构造出来  Hilbert 空间的一组标准正交基，那么对于有限维空间 $\text{dim}H=n$，其与 $\mathbb{K}^n$ 等距同构；对于无穷维可分空间，其与 $\ell^2$ 等距同构。

> **小结**：
>
> 1. 本小节讲到的标准正交集可以用于向量的线性分解表示。
> 2. 当标准正交集是完全的，那么它实际上就是 Hilbert 空间 $H$ 的一组标准正交基。这又可以看作是线性空间中 Hamel 基/Schauder 基的更特殊的情况，因为完备的标准正交集要求相互正交，而 Hamel 基和 Schauder 基只要求线性无关。
> 3. 实际上根据 Gram-Schmidt 标准正交化方法可以由两种基构造出标准正交基。



## 4. Hilbert 空间上有界线性泛函表示

对于内积空间 $X$，取 $z_0\in X$，那么我们可以定义一个线性泛函 $f_{z_0}(x)=\langle x,z_0\rangle$，可以验证 $f_{z_0}\in X^{\star}$，并且有 $\Vert f_{z_0}\Vert = \Vert z_0\Vert$，因此 $f_{z_0}\in X'$。

那么如果反过来，任取 $f\in X'$，我们是否能够断言 $f$ 都可以表示为 $f(x)=\langle x,z_0\rangle$ 的形式呢？可以！这个表示形式如此简洁，以后你会发现这个性质非常有用！但并不是任意赋范空间都可以如此表示，只有 Hilbert 空间有这个良好的性质。

> **定理(F.Riesz)**：$H$ 为 Hilbert 空间，则 $\forall f\in H',\exists! z_0\in H,f(x)=\langle x,z_0\rangle .$
>
> **证明**：若 $f=0$，取 $z_0=0.$
>
> 若$f\ne0,f\in H'$，那么 $N(f)=\{x\in H,f(x)=0\}$ 为 $H$ 的闭线性子空间，因此有 $H=N(f)\oplus N(f)^{\perp}$，且 $N(f)^{\perp}\ne\{0\}$。接下来的问题就是如何构造一个函数 $\langle x,z\rangle $ 让其等于 $f(x)$？
>
> 首先固定 $z_0\in N(f)^{\perp},z_0\ne0$。任给 $x\in H$，考虑 $v=f(x)z_0-f(z_0)x \in H$，显然有 $f(v)=0$，故 $v\in N(f)$，从而有
> $$
> \begin{aligned}
> \langle v,z_0\rangle  = f(x)\langle z_0,z_0\rangle -f(z_0)\langle x,z_0\rangle =0 \\
> \Longrightarrow f(x)=\langle x,\frac{\overline{f(z_0)}z_0}{\Vert z_0\Vert^2}\rangle =\langle x,y_0\rangle .
> \end{aligned}
> $$
> 下面证明 $y_0$ 的唯一性。略。
>
> 证毕。
>
> **NOTE**：实际上 $N(f)^{\perp}$ 是一维空间，也就是 $\text{span}\{z_0\}$，因此我们随便从 $N(f)^{\perp}$ 中取一个向量 $z_0$ 乘以一个线性系数就能得到 $f(x)$。
>
> >  显然 $g(x)=\langle x,z_0\rangle $ 并不一定等于 $f(x)$，那么我们怎么让他变化一下变成 $f$ 呢？看来需要对 $g$ 做一些映射，最简单的做一个线性变换，考虑 $g'(x)=cg(x)$，要想让 $g'(x)=f(x)$，至少应该满足 $g'(z_0)=c\langle z_0,z_0\rangle =f(z_0)\Rightarrow g'(x)=\langle x,\frac{\overline{f(z_0)}z_0}{\Vert z_0\Vert^2}\rangle =\langle x,y_0\rangle .$ 那么我们来验证一下是否任意的 $x\in H$ 都有 $g(x)=f(x)$？
> >
> >  $z_0\in N(f)^{\perp},\forall x\in N(f),g'(x)=0$ 没毛病，

假设 $X,Y$ 为 $\mathbb{K}$ 上的赋范空间，称 $f:X\times Y\to \mathbb{K}$ 为**共轭双线性泛函**，若
$$
\begin{aligned}
f(\lambda_1 x_1+\lambda_2 x_2,y)=\lambda_1 f(x_1,y)+\lambda_2 f(x_2,y) \\
f(x,\lambda_1 y_1+\lambda_2 y_2)=\overline{\lambda_1} f(x,y_1)+\overline{\lambda_2} f(x,y_2)
\end{aligned}
$$
称其为有界的，若 $\exists C\ge0$，使
$$
|f(x,y)|\le C\Vert x\Vert \Vert y\Vert, \quad \forall x\in X, y\in Y
$$
定义其范数为
$$
\Vert f\Vert = \sup_{x\ne0,y\ne0} \frac{|f(x,y)|}{\Vert x\Vert \Vert y\Vert}.
$$
**定理(F.Riesz)**：$H_1,H_2$ 为 Hilbert 空间，$f:H_1\times H_2\to \mathbb{K}$ 为有界共轭双线性泛函，则 $\exists! S\in B(H_1,H_2), f(x,y)=\langle Sx,y\rangle ,x\in X,y\in Y.$

证明：首先固定 $x\in H_1$，只关注 $H_2 \to \mathbb{K}$，即 $y\mapsto \overline{f(x,y)}$ 为有界线性泛函，因此可以 $\exists!S_x\in H_1$，使得
$$
\overline{f(x,y)}=\langle y,S_x\rangle \Rightarrow f(x,y)=\langle S_x,y\rangle 
$$
现在对 $x$ 任取，可以得到 $S:H_1\to H_2$，由于 $f$ 是有界共轭双线性的，容易验证 $S$ 为有界线性算子。证毕。

下面如果我们定义 $g(x,y)=\overline{f(y,x)}=\langle x,Sy\rangle ,x\in H_2,y\in H_1$，那么 $g$ 也是有界共轭双线性算子，并且根据上面的 Riesz 表示定理，$\exists! T\in B(H_2，H_1)$ 使得 $g(x,y)=\langle Tx,y\rangle =\langle x,Sy\rangle .$ 由此我们就导出了伴随算子的定义，我们称 $S$ 为 $T$ 的**伴随算子**，记为 $S=T^{\star}$，并且根据上面的推导过程可以得到伴随算子是**唯一的**。

伴随算子有以下性质：

- $\Vert T^{\star}\Vert=\Vert T \Vert$
- $(\lambda S+\mu T)^{\star}=\bar{\lambda}S^\star + \bar\mu T^{\star}$
- $(T^\star)^\star=T$
- $\Vert T^{\star}T\Vert=\Vert TT^{\star}\Vert=\Vert T \Vert^2$
- $T^{\star}T=0 \iff T=0$
- $(PS)^\star=S^\star P^\star$

*例子 1*：伴随算子一个最简单的例子就是矩阵。$H_1=H_2=\mathbb{K}^n,T\in B(H_1,H_2),\exists!A$ 为 $n$ 阶方针，使得 $Tx=Ax$，容易验证 $T^\star x=A^H x.$

**定理**：$H_1,H_2$ 为 Hilbert 空间，$T\in B(H_1,H_2)$ 为一一映射，则 $T$ 为**等距同构**，当且仅当
$$
TT^\star=I_{H_2},\quad T^\star T=I_{H_1.}
$$
此时称 $T$ 为 $H_1$ 到 $H_2$ 的**酉算子**。

证明：要证明等距同构只需要验证 $\langle Tx,Ty\rangle _2=\langle x,y\rangle _1,\forall x,y\in H_1$ 或者 $\langle T^\star x,T^\star y\rangle _1=\langle x,y\rangle _2,\forall x,y\in H_2$ 成立，做一下变换即可证明。证毕。

**NOTE**：实际上这里可以联想到酉矩阵。

> **小结**：这一小节最核心的内容就是 Riesz 表示定理，即 **Hilbert 空间**上任意**有界线性泛函**都可以**唯一地**表示为
> $$
> f(x) = \langle x,z_0\rangle , \forall x\in H
> $$

