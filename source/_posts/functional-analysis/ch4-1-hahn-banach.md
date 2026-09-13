---
title: 泛函分析笔记4：Hahn-Banach定理
date: 2020-12-14 11:38:39
tags:
  - Hilbert空间
  - Hahn-Banach定理
  - 次线性泛函
  - 半范
categories:
  - Functional Analysis
mathjax: true
---

前面三章讲了很多东西，但实际上都只是开胃小菜 >__<，度量空间、赋范空间、内积空间等等都是为了我们接下来要讲的四大定理做铺垫。泛函中的四大定理，即 Hahn-Banach 定理、一致有界性原理、开映射定理和闭图像定理，是整个泛函分析的基石（前面三章的内容是基石的基石）。

<!--more-->

## 1. Hahn-Banach 定理

首先介绍几个要用到的概念。对于定义了序关系 $\preceq$ 的集合 $X$，$N\subset X$，称 $x_0\in X$ 为 $N$ 的**上界**，若 $x\preceq x_0,\forall x\in N$；称 $y_0\in N$ 为**极大元**，若 $y_0\le y,\forall y\in N \Longrightarrow y=y_0.$

**Zorn引理**：非空半序集合 $(X,\preceq)$，若 $X$ 的任意非空全序子集均有上界，则 $X$ 必有极大元。

Hahn-Banach 定理有很多种不同的形式，在给出第一个 Hahn-Banach 定理之前，我们先引入次线性泛函。对于线性空间 $X$，称 $p:X\to \mathbb{K}$ 为**次线性泛函**，若：

1. $\forall x,y\in X, p(x+y)\le p(x)+p(y)$
2. $\forall x\in X, a\ge0,p(ax)=ap(x).$

> **定理(Hahn-Banach) 1**：对于**实线性空间** $X$，$X_0$ 为 $X$ 的线性子空间，$p$ 为 $X$ 上的**次线性泛函**，$f_0\in X_0^\star$ 为线性泛函并且满足 $f_0(x)\le p(x),\forall x\in X_0$，那么存在 $f\in X^\star$ 使得 $f|_{X_0}=f_0$，并且 $f(x)\le p(x),\forall x\in X.$

**证明**：要证明 $f$ 的存在性我们就需要找到满足条件的 $f$，但是给出的 $f_0$ 只在 $X_0$ 上有定义，怎么把它扩展到整个 $X$ 上得到我们想要的 $f$ 呢？下面的构造方法非常的“山路十八弯”，我自己是想不到 orz。

首先定义 $\mathcal{E}=\{(Y,g): Y为X的线性子空间,X_0\subset Y,\quad g\in Y^\star ,g|_{X_0}=f_0, g(x)\le p(x),\forall x\in Y\}$ （这个集合的构造就不是一般人能想到的[狗头]）。由于 $(X_0,f_0)\in \mathcal{E}$，因此 $\mathcal{E}$ 非空，我们可以定义 $\mathcal{E}$ 上的序关系 
$$
(Y_1,g_1)\preceq(Y_2,g_2) \iff Y_1\subset Y_2,\ g_2|_{Y_1}=g_1
$$
那么取 $\mathcal{E}$ 的非空全序子集 $\mathcal{E}_1$，令 $W=\cup_{(Y,g)\in \mathcal{E}_1}Y$，对应的 $\forall x\in W$，我们可以找到 $(Y,g)\in \mathcal{E}_1, x\in Y$，此时定义 $h(x)=g(x)$。容易验证 $W$ 为 $X$ 的线性子空间，另外由于 $\mathcal{E}_1$ 是全序的，也可以验证 $h(x),\forall x\in W$ 的定义是唯一的（与 $(Y,g)$ 的选取无关），并且 $h(x)$ 为 $W$ 上的线性泛函。因此我们有 $(W,h)\in\mathcal{E}.$

由于任意 $(Y,g)\in\mathcal{E}_1$，都有 $(Y,g)\preceq(W,h)$，即 $(W,h)$ 为 $\mathcal{E}_1$ 的上界，$\mathcal{E}$ 中任意非空全序子集都有上界，根据 Zorn 引理，$\mathcal{E}$ 有极大元 $(Y_0,g_0).$

到这里我们就把 $(X_0,f_0)$ 扩展到了一个更大的空间 $(Y_0,g_0)$ 上面，但是 $Y_0$ 是否就是 $X$ 呢？感觉应该是，但是需要证明一下。

假如 $Y_0\subsetneq X$，可以取 $y\in Y_0^c$，考虑 $V=\text{span}(Y_0\cup \{y\})$，那么 $\forall x\in V$ 都可以唯一的表示为 $x=w+\lambda y,w\in Y_0,\lambda\in\mathbb{R}$，此时定义
$$
h(x)=g_0(w)+\lambda a
$$
容易验证 $h$ 为 $V$ 上的线性泛函，并且有 $h_{Y_0}=g_0$，因此我们就对 $g_0$ 又进行了扩展，只需要证明 $(V,h)\in\mathcal{E}$ 就能导出矛盾（因为已经证明 $(Y_0,g_0)$ 为极大元）从而说明 $Y_0=X$。这就需要满足 $h(x)\le p(x),\forall x\in V$。考虑 $\lambda>0$，这等价于 $h(x/\lambda)=g_0(w/\lambda)+a\le p(w/\lambda+y)$，也等价于 $\forall w\in Y_0, g_0(w)+a\le p(w+y)$。类似得取 $x=w-\lambda y,\lambda>0$ 就可以得到 $g_0(w)-a\le p(w-y)$。因此 $a$ 需要满足以下条件：
$$
g_0(w)-p(w-y) \le a \le p(w+y) - g_0(w)
$$
因此就需要满足 $2g_0(w) \le p(w-y)+p(w+y)$，由于 $2g_0(w)\le p(2w) \le p(w+y+w-y)\le p(w+y)+p(w-y)$，因此上式一定可以满足，即能找到合适的 $a$ 使得 $(V,h)\in\mathcal{E}$，因为 $Y_0\subsetneq V$，这就说明 $(Y_0,g_0)$ 不是 $\mathcal{E}$ 的极大元，导出矛盾，因此有 $Y_0=X$。至此我们就找到了 $g_0$ 满足定理要求的全部条件。

证毕。

上面的定理只研究了实线性空间的情况，对于复线性空间有相似的结论，不过我们需要对条件稍加修改。

首先将上面 $p$ 的次线性泛函假设改为更强的假设——半范。对于线性空间 $X$，$p:X\to\mathbb{R}$ 称为 $X$ 上的**半范**，若满足：

1. $p(x)\ge0, \forall x\in X$
2. $p(x+y)\le p(x)+p(y),\forall x,y\in X$
3. $p(ax)=|a|p(x),\forall x\in X,a\in\mathbb{K}$

半范与范数的概念相比，少了一个条件即 $p(x)=0\iff x=0$，因此说明可能存在 $x\ne0,p(x)=0$。另外半范可以导出次线性。

> **定理(Hahn-Banach) 2**：$X$ 为 $\mathbb{K}$ 上的线性空间，$X_0$ 为 $X$ 的线性子空间，$p:X\to \mathbb{R}$ 为半范，$f_0\in X_0^\star$ 并且有 $|f_0(x)|\le p(x),\forall x\in X_0$，那么存在 $f\in X^\star$ 使得 $f|_{X_0}=f_0$，并且 $f(x)\le p(x),\forall x\in X.$

**证明**： 要想把原来的结论从实线性空间扩展到复线性空间，首先要考虑复线性空间有什么特殊的性质。如果用两个实线性泛函来表示 $f(x)=f_1(x)+if_2(x)$，我们可以想一下复线性空间可以粗略的看成两个实线性空间的组合，$\forall a\in\mathbb{R},f(ax)=af_1(x)+iaf_2(x)$。但是实际上并没有这么简单，因为 $a$ 还可以取自复空间，那么我们令 $a=i$ 就可以得到
$$
\begin{aligned}
if(x)&=f(ix) \\
if_1(x)-f_2(x)&=f_1(ix)+if_2(x)
\end{aligned}
$$
由于 $x\in X$ 任取，实部虚部需要分别相等，因此 $f_2(x)=-f_1(ix),\forall x\in X$，即**复线性泛函 $f$ 的实部和虚部是相互约束的**！另一方面，实部和虚部只需要知道其中一个，另一个也就确定了。那么也就是说给定一个复线性泛函 $f$ 我们可以唯一的用某个实线性泛函 $f_1$ 来表示 $f$。

> **NOTE**：读者如果了解信号的相干解调的话，这里可以联想到 Hilbert 变换。实际上。

对于定理中的 $f_0$ 我们可以将其表示为 $f_0(x)=f_1(x)+if_2(x)$。容易验证 $f_1(x)\le p(x)$，根据上一个 Hahn-Banach 定理，就能找到一个实线性泛函 $g_1$，使得 $g_1|_{X_0}=f_1, g_1(x)\le p(x)$。那么我们再定义 $g(x)=g_1(x)-ig_1(ix)$，容易验证 $g\in X^\star,g|_{X_0}=f_0$，接下来验证 $|g(x)|\le p(x)$ 的方法也有一点巧妙：
$$
|g(x)|=g(x)e^{-i\theta} = g(e^{-i\theta}x)=g_1(e^{-i\theta}x)\le p(e^{-i\theta}x)=p(x)
$$
证毕。

接下来再把 $X$ 限制在赋范空间上，就能得到有界线性泛函的保范延拓定理，这也是 Hahn-Banach 定理最重要的应用。我们这里讲前面两个定理基本上也是为下面一个定理做铺垫。他们的基本含义都是将一个线性泛函从较小的线性子空间延拓到整个空间。

> **定理(Hahn-Banach/保范延拓定理) 3**：$(X,\Vert\cdot\Vert)$，$X_0$ 为 $X$ 的线性子空间，$\forall f_0\in X_0'$，则 $\exists f\in X'$，$f|_{X_0}=f_0$ 且 $\Vert f\Vert = \Vert f_0\Vert.$

**证明**：我们还是要对 $f_0$ 进行延拓，那么怎么样才能应用上面两个定理的结论呢？首先需要构造一个半范 $p$，可以定义 $p(x)=\Vert f_0\Vert\cdot \Vert x\Vert$，容易验证其为半范（在这里能看出来半范的好处，由于不要求 $p(x+y)=p(x)+p(y)$，因此 $p$ 可以含有 $\Vert x\Vert$ 项）。那么就能找到 $f\in X^\star,f|_{X_0}=f_0,|f(x)|\le \Vert f_0\Vert\cdot \Vert x\Vert$，之后容易验证 $\Vert f\Vert=\Vert f_0\Vert.$ 证毕。

对上一定义的假设条件进行增强，把 $X$ 限制为 Hilbert 空间，回忆上一章提到了 **Hilbert 空间**上的**线性泛函**都可以**唯一的**表示为 $f(x)=\langle x,z_0\rangle$ 的形式，那么我们可以把上面的保范延拓定理进一步加强，获得唯一性！

> **推论 1**：若 $X$ 为 HIlbert 空间，$X_0$ 为 $X$ 的闭线性子空间，$f_0\in X_0'$，则**存在唯一的** $f\in X'$，$f|_{X_0}=f_0$ 且 $\Vert f\Vert = \Vert f_0\Vert.$

**证明**：$X_0$ 也是 Hilbert 空间，因此 $\exists! z_0\in X_0$，使得 $f_0(x)=\langle x,z_0\rangle$，因此很方便地可以取 $f_0(x)=\langle x,z_0\rangle, \forall x\in X$，并且可以验证 $f$ 满足条件 $f|_{X_0}=f_0, \Vert f\Vert = \Vert f_0\Vert=\Vert z_0\Vert.$ 

接下来就需要证明 $f$ 的唯一性。假设还有 $g$ 满足条件，那么 $\exists! z_1\in X$，$g(x)=\langle x,z_1\rangle$
$$
\forall x\in X_0.f(x)-g(x)=\langle x,z_0-z_1\rangle=0 \Longrightarrow z_0-z_1\in X_0^{\perp} \\
\Vert g_0\Vert=\Vert z_1\Vert^2 = \Vert z_1-z_0\Vert^2+\Vert z_0\Vert^2=\Vert f_0\Vert^2 \Longrightarrow z_1=z_0
$$
证毕。

> **定理(Hahn-Banach) 4**：$(X,\Vert\cdot\Vert)$，$x_0\in X, x_0\ne 0$，则 $\exists f\in X', \Vert f\Vert=1$，使得 $f(x_0)=\Vert x_0\Vert.$

**证明**：可以取 $X_0=\mathbb{K}x_0=\{\lambda x_0,\lambda\in\mathbb{K}\}$，定义 $f_0(\lambda x_0)=\lambda \Vert x_0\Vert \in X_0'$，$\Vert f_0\Vert=1$，再应用上一推论自然就出来了。证毕。

**NOTE**：对于 $\forall x,y\in X,x\ne y$，那么我们取 $x_0=x-y\ne0$，根据上面的定理，一定存在 $f\in X'$ 使得 $f(x_0)=f(x)-f(y)\ne0$。这说明只要 $x\ne y$，就一定存在某个 $f\in X',\Vert f\Vert=1$ 使得 $f(x)\ne f(y).$ 可以表述为
$$
x=y \iff f(x)=f(y),\forall f\in X'
$$

> **推论 2**：$X\ne\{0\}$ 为赋范空间，$x_0\in X,x_0\ne 0$，则
> $$
> \Vert x_0\Vert = \max_{f\in X',\Vert f\Vert=1} |f(x_0)| = \max_{f\in X',\Vert f\Vert\le1} |f(x_0)| = \max_{f\ne0} \frac{|f(x_0)|}{\Vert f\Vert}
> $$
> **NOTE**：回忆 $\Vert f\Vert=\sup_{x\in X,\Vert x\Vert\le1}|f(x)|$，从这个结论来看，$X$ 与 $X'$ 有某种的对称性。证明略。



