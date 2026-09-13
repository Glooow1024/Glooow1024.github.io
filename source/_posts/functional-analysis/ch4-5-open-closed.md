---
title: 泛函分析笔记8：开映射定理与闭图像定理
toc: true
date: 2020-12-27 11:21:40
tags:
  - 开映射定理
  - 闭图像定理
  - 开映射
  - 闭算子
categories:
  - Functional Analysis
mathjax: true
---

接下来四大定理就剩下了两个：开映射定理与闭图像定理。剩下的内容相比于前面两个定理要少很多，也更简单，我们一口气讲完吧！

## 1. 开映射定理

$(X_1,d_1),(X_2,d_2)$，称 $T:X_1\to X_2$ 为**开映射**，若 $\forall G\subset X$ 为开集，都有 $T(G)$ 在 $X_2$ 中为开集。

**NOTE**：我们讲到连续映射的时候证明了一个定理，即 $T$ 为**连续映射** $\iff \forall G\subset X_2$ 为开集，那么 $T^{-1}(G)=\{x\in X_1, Tx\in G\}$ 是 $X_1$ 中的开集。注意这与开映射不同，开映射是正向的，连续映射是反向的。

<!--more-->

在给出开映射定理之前我们需要先准备几条引理。

**引理**：设 $X$ 为赋范空间，$\lambda\in\mathbb{K},\lambda\ne0$

- 若 $x_0\in X,r>0$，则 $\lambda B(x_0,r) = B(\lambda x_0,|\lambda|r)$；
- 若 $M\subset X$，则 $\overline{\lambda M}=\lambda \overline{M}$；
- 若 $M\subset X$，则 $(\lambda M)^\circ=\lambda M^\circ$。

证明：只证第3条，若 $x\in (\lambda M)^\circ$，那么存在 $r>0，B(x,r)\in \lambda M$，也即 $\frac{1}{\lambda} B(x,r)\in M$，根据第1条结论，也即 $B(x/\lambda,r/|\lambda|)\in M$，因此 $x/\lambda\in M^\circ$，所以有 $(\lambda M)^\circ\subset\lambda M^\circ$。反过来若 $x\in\lambda M^\circ$，即 $x/\lambda\in M^\circ$，类的的可以得到 $\lambda M^\circ\subset(\lambda M)^\circ.$ 证毕。

**引理**：$X,Y$ 为 Banach 空间，$T\in B(X,Y)$ 为满射，则 $T(B_X(0,1))$ 包含一个以 $0$ 为中心的开球，即存在 $\delta>0$，$B_Y(0,\delta)\subset T(B_X(0,1)).$

**证明**：这个引理看起来简单，证明还挺复杂的。我们记 $B_n=B_X(0,1/2^n)$。

首先证明 $\overline{T(B_1)}$ 包含某个开球。考虑到 $\bigcup_{n=1}^\infty nT(B_1)=Y$，由于 $Y$ 为 Banach 空间，也可以表示为 $\bigcup_{n=1}^\infty n\overline{T(B_1)}=Y$，根据 Baire 范畴定理，一定存在 $n_0\ge1$ 使得 $n_0\overline{T(B_1)}$ 有内点，即存在 $y\in Y,\delta>0$ 使得 $B(y,\delta)\subset n_0\overline{T(B_1)}$，记 $B(y_0,\delta_0)=B(y/n_0,\delta/n_0)\subset \overline{T(B_1)}$。

接下来证明 $B(0,\delta_0)\subset \overline{T(B_0)}$。对任意 $y\in B(0,\delta_0)$，有 $y_0+y\in B(y_0,\delta_0)$，所以存在 $u_n,v_n\in B_1$ 分别有 $Tu_n\to y_0,Tv_n\to y_0+y$，此时 $v_n-u_n\in B_0$，并且 $T(v_n-u_n)\to y$，因此 $y\in \overline{T(B_0)}$。

最后证明 $B(0,\delta_0/2)\subset T(B_0)$。已有 $B(0,\delta_0/2^n)\subset \overline{T(B_n)}$，任取 $y\in B(0,\delta_0/2)$，由于 $B(0,\delta_0/2)\subset \overline{T(B_1)}$，因此存在 $x_1\in B_1$ 使得 $\Vert y-Tx_1\Vert < \delta_0/2^2$。因此 $y-Tx_1\in B(0,\delta_0/2^2)$，又由于 $B(0,\delta_0/2^2)\subset \overline{T(B_2)}$，因此存在 $x_2\in B_2$ 使得 $\Vert y-Tx_1-Tx_2\Vert < \delta_0/2^3$。依此类推，对任意的 $n\ge1$，可以找到 $x_n\in B_n$，使得
$$
\Vert y-Tx_1-Tx_2-\cdots-Tx_n\Vert < \frac{\delta_0}{2^{n+1}}
$$
若令 $s_n=x_1+\cdots+x_n$，则有 $s_n$ 为柯西列，假设 $s_n\to s\in X$，则可以验证 $y=Ts$。因为 $\Vert s\Vert\le \sum_{n=1}^\infty \Vert x_n\Vert<1$，因此 $s\in B_0$，所以 $y\in T(B_0)$，故 $B(0,\delta_0/2)\subset T(B_0)$。证毕。

> **开映射定理**：$X,Y$ 为 Banach 空间，$T\in B(X,Y)$ 为满射，则 $T$ 一定是开映射。

证明：由前面的引理很容易证明，略。

> **推论**：$X,Y$ 为 Banach 空间，$T\in B(X,Y)$ 为满射，特别地，若 $T$ 为一一映射，我们有 $T^{-1}\in B(Y,X)$（即逆映射也是有界的）。

证明：由于 $T$ 为开映射且为一一映射，因此 $T^{-1}$ 为连续的，因此 $T^{-1}\in B(Y,X)$。

**推论**：$(X,\Vert\cdot\Vert_1),(X,\Vert \cdot\Vert_2)$ 均为 Bananch 空间，若存在常数 $\alpha>0$ 使得 $\Vert x\Vert_1\le \alpha\Vert x\Vert_2,\forall x\in X$，则存在 $\beta>0$ 使得 $\Vert x\Vert_2\le \beta\Vert x\Vert_1,\forall x\in X$，即 $\Vert\cdot\Vert_1$ 与 $\Vert\cdot\Vert_2$ 为等价范数。

证明：考虑 $T:(X,\Vert\cdot\Vert_2)\to(X,\Vert \cdot\Vert_1)$，有 $T:x\mapsto x$。根据条件可以知道 $T$ 为有界线性算子，并且 $T$ 为一一映射。因此 $T^{-1}$ 也是有界线性算子，$\Vert x\Vert_2=\Vert T^{-1}x\Vert_2\le \Vert T^{-1}\Vert\Vert x\Vert_1,\forall x\in X$。证毕。



## 2. 闭图像定理

设 $X,Y$ 为赋范空间，令 $Z=X\times Y=\{(x,y): x\in X,y\in Y \}$，定义 $\Vert(x,y)\Vert=\Vert x\Vert_X+\Vert y\Vert_Y$，可以验证 $\Vert\cdot\Vert$ 为 $Z$ 上的范数。若 $X,Y$ 为 Banach 空间，则 $Z$ 也为 Banach 空间。

设 $X,Y$ 为赋范空间，$D(A)$ 为 $X$ 中的线性子空间，$A:D(A)\to Y$ 为线性算子，称 $A$ 为**闭算子**，若 $G_A = \{(x,Ax)\in X\times Y:x\in D(A) \}$ 为闭集。

**命题**：有界线性算子 $\Rightarrow$ 闭算子。

证明：有界线性算子 $A$，任意 $(x,Ax)\in G_A$，存在 $x_n\to x$，假设 $(x_n,Ax_n)\to (x,y)$，由于 $A$ 连续，因此 $Ax_n\to Ax$，因此 $y=Ax$，即 $(x,y)\in G$，因此 $G_A$ 为闭集。证毕。

***例子 1***(闭算子 $\nRightarrow$ 有界线性算子)：考虑求导算子，$X=Y=C[0,1], \Vert x\Vert_\infty=\max_{0\le t\le1}|x(t)|$。$D(A)=C^1[0,1]$，$A:x\mapsto x'$ 为线性算子，考虑 $x_n(t)=t^n, \Vert x\Vert_\infty=1$，有 $\Vert Ax_n\Vert=n$，因此 $A$ 无界，但是 $A$ 为闭算子。

证明：假设 $(x_n,x_n')\to (x,y)$，即 $x_n\rightrightarrows x,x_n'\rightrightarrows y$，现证明 $y=x'$。由于 $x_n' \rightrightarrows y$ 一致收敛，积分与极限可以换序，对于 $t\in[0,1]$ 有
$$
\begin{aligned}
\int_0^t y(s)ds &= \int_0^t \lim_{n\to\infty} x_n'(s)ds=\lim_{n\to\infty}\int_0^t x_n'(s)ds \\
&=\lim_{n\to\infty} x_n(t)-x_n(0) = x(t)-x(0)
\end{aligned}
$$
由于 $y\in C[0,1]$，因此 $\int_0^t y(s)ds$ 为连续可导函数，因此 $x\in C^1[0,1]=D(A)$，并且有 $x'=y$，这说明 $(x,y)=(x,x')\in G_A$，从而 $A$ 为闭算子。

**NOTE**：闭算子的定义域 $D(A)$ 不一定是闭集，比如上面的求导算子。

***例子 2***(有界、不闭算子)：$X$ 为赋范空间，$X_0\subsetneq X$ 为线性子空间，$X_0$ 不闭。算子 $A:X_0\to X(x\mapsto x)$ 为线性有界算子。但是对于 $y\in \bar{X}\backslash X_0$，存在 $y_n\in X_0$ 使得 $(y_n,Ay_n)\to (y,y)\notin G_A$，因此 $A$ 不是闭算子。



> **闭图像定理**：$X,Y$ 为 **Banach 空间**，$D(A)$ 为 $X$ 的线性子空间，$A:D(A)\to Y$ 为**闭算子**。若 $D(A)$ 为 $X$ 的**闭线性子空间**，则 $A$ **有界**。

**证明**：证明算子有界性可以想到一致有界性原理，但是这里并没有说 $D(A)$ 可分，也没有说 $X$ 自反，因此一致有界性原理行不通。那么又可以联想到前面开映射定理，若有界算子 $T$ 为开映射，同时又是一一映射，那么 $T^{-1}$ 也是有界算子。因此我们构造 $P:G_A\to D(A)((x,Ax)\mapsto x)$，其中 $G_A$ 为 Banach 空间，并且 $P$ 为有界线性算子，为满射、一一映射，因此 $P^{-1}:D(A)\to G_A(x\mapsto (x,Ax))$ 也是有界算子，进而 $A$ 也有界。

> **推论**：$X,Y$ 为 **Banach 空间**，$D(A)=X$，若 $A:X\to Y$ 为**闭算子**，则 $A\in B(X,Y).$
>
> **NOTE**：如果想直接证明 $A\in B(X,Y)$ 需要什么条件？$\forall x_n\in X$, 设 $x_n\to x$，则 1）$Ax_n$ 收敛；2）收敛极限为 $Ax$。但如果我们有 $A$ 为闭算子的条件，那么不需要证明 $Ax_n$ 收敛，可以直接假设 $Ax_n$ 收敛到某 $y$，只需要证明 $y=Ax$ 即可。
>
> 这是因为我们已经假设了 $X,Y$ 均为 Banach 空间，因此 $X\times Y$ 也一定是 Banach 空间，所以一定有 $(x_n,Ax_n)$ 收敛到某 $(x,y)$，因此 $Ax_n$ 也一定收敛。

*例子 3*：$H$ 为 Hilbert 空间，$A:H\to H, B:H\to H$ 均为线性算子，并且满足 $\forall x,y\in H$，$\langle Ax,y\rangle=\langle x,By\rangle$，则 $A,B$ 均有界。

证明：$\langle Ax_n,y\rangle=\langle x_n,By\rangle\to\langle x,By\rangle=\langle Ax,y\rangle$，由于 $y$ 任取，因此 $Ax_n\to Ax$，所以 $A$ 为闭算子，因此 $A$ 有界。证毕。