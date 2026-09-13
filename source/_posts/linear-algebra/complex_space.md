---
title: 转载|理解复数域上的向量空间
date: 2021-04-08 11:23:30
tags:
  - 复数域空间
  - 矩阵分析
  - 转载
categories: 
  - Linear Algebra
mathjax: true
---

> 说明：本文转载自[博客：理解复数域上的向量空间（第一篇）](https://math.tianpeng.org/2010/07/%e7%90%86%e8%a7%a3%e5%a4%8d%e6%95%b0%e5%9f%9f%e4%b8%8a%e7%9a%84%e5%90%91%e9%87%8f%e7%a9%ba%e9%97%b4%ef%bc%88%e7%ac%ac%e4%b8%80%e7%af%87%ef%bc%89/)，由于没有找到原作者的联系方式，所以直接搬运过来了，如有侵权请联系我，我立即删除。

线性代数进行到酉空间中的自伴算子、正规算子以及谱定理这部分内容时，会发现很多在复空间中成立的命题在实空间中却未必成立。这种情况多少让人感到有点奇怪，为什么会出现这种情况？
 复数域是包含实数域的，我们学习复数之后碰到最多的是相反的情况：原本在实数域上成立的性质在复数域中不一定成立了，比如，实数可以比较大小，但复数没有大小关系；又比如，实数的平方非负，等等。这样的命题见多了，容易使人产生思维定势，认为复数包含实数，因此在复数范围内成立的命题在实数范围内也必然成立，而实数范围成立的命题不一定都能推广到复数。
 可尤其是学习到复变函数之后，这种情况似乎反过来了，同样的一个概念，到了复数中反倒比原来实数情况下的相应概念有了更多的内涵。这又是为什么呢？

<!--more-->

比如，在”Linear Algebra Done Right” 第七章有个命题 7.2，是说

**命题7.2：** 如果 $V$是复数域上的内积空间，并且 $T$是 $V$上的线性算子，且对任意向量 $v$，都有 $\langle Tv,v\rangle=0$，那么 $T=0$。
 **证明：** 使用恒等式
 $\begin{aligned}\langle Tu,w\rangle=&\frac{\langle  T(u+w),u+w\rangle-\langle T(u-w),u-w\rangle}{4}\\ &+\frac{\langle  T(u+iw),u+iw\rangle+\langle T(u-iw),u-iw\rangle}{4}i\end{aligned}$
 即可得证。

但是，同样的假设，在实数空间中却得不出同样的结论来，比如，二维空间中把所有向量都逆时针旋转90度角。

可是，在实空间中可以存在旋转90度的映射，为什么在复空间中就没有这种映射？难道就不可以有一个线性变换像实空间中那样把每一个向量都旋转到垂直的位置上吗？



它的证明的确在那里，证法也的确没有错，但是我却从直观上难以接受这个命题。我不甘心，于是找来实空间中的那个旋转映射放在复空间上，定义复空间上的映射
 $T\begin{pmatrix}a\\ b\end{pmatrix}=\begin{pmatrix}b\\ -a\end{pmatrix}$
 那么 $\langle Tv,v\rangle=a\bar b-b\bar a$，通常情况下，这是个纯虚数，不一定是零。所以这个反例失败了，在这个例子上，我不得不承认上面那个命题。但是，它给我带来更多的思考：

我们应该怎样直观地想象复向量空间？从前我们总是把向量想象成一条带有方向的线段，把一维子空间想象成一条直线，”线”总是伴随着我们对向量空间的影像理解，因为实数上面可以定义线序，而我们生存的三维空间就可以看成实三维空间，所以在理解一般的向量空间的时候，这种影像可以帮助我们建立起很多问题的直观。但是，当探究复向量空间内部的特殊结构的时候，这种形象的理解遇到了一些问题。
 比如，把一个复向量取共轭，跟原来的向量是什么位置关系？又比如，一个向量 $v$乘以 $i$，变成了 $iv$，是把这个向量怎么样了？旋转了九十度？可是毕竟二者是线性相关的，而在我们的直观理解中，线性相关的两个向量是共线的，数乘只是把向量拉长或缩短。但是如果把它们两个想象成共线的，又无法想象它们两个有相同的长度，一个一维子空间中相同长度的向量不是两个，而是无穷多个，无法想象。
 看来，与复平面类似，只有把一维复向量空间理解成我们所熟悉的平面才自然一些，而这个一维复空间中的一个复向量，可以看成是躺在个平面上的，它并不占据这个一维空间的一整段，在它周围可以有无数多条与它长度相等的向量。
 那么这么看来，一个 n 维的复空间，是否可以形象地把它想象成一个 2n 维实空间？

把问题提得更明确一些，如果我们把一个 n 维复空间看成一个实数域上的向量空间，即把数量乘法中用到的数限制在实数域中，那么，原来的基底 $e_1,e_2,\dots,e_n$就张不成整个空间了，可以证明， 向量组 $e_1,ie_1,e_2,ie_2,\dots,e_n,ie_n$是这个实向量空间的基底，一共 2n 个元素，所以它是一个 2n  维实向量空间。（注意，这个实数空间中的一个向量 v 在原来复空间意义下可以乘以纯量 i 变成 iv，但在实空间中 iv  将不再是这种乘法的结果，它只是与 v 有关的一个向量而已。我们仍然记为 iv。）

如果只考虑空间本身，那么这样的理解是足够的，也足以提供复空间的一些直观信息。可是，我们需要理解的不止是这些，还有内积、线性变换等一些东西，在这个衍生出来的实空间中的这些东西到底跟原来的复空间中相应的对象有什么联系？

首先分析内积，如果原来的复空间中有内积 $\langle \cdot,\cdot\rangle$，那么 $\mathrm{Re}\,\langle \cdot,\cdot\rangle$就是衍生出的实空间中的内积，我们用方括号 $[\cdot,\cdot]$表示这个实空间的内积，
 那么原来的内积就可以用这个实数空间内积表示为
 $\langle u,v\rangle=[u,v]+i[u,iv]$
 并且，这两个内积诱导的范数是相同的。
 对任意向量 $v$，向量 $iv$在实数空间的内积意义下是垂直于 $v$的： $[iv,v]=\mathrm{Re}\,i\langle v,v\rangle=0$。
 如果 $e_1,e_2,\dots,e_n$是复空间的标准正交基底，那么 $e_1,ie_1,e_2,ie_2,\dots,e_n,ie_n$是实空间的标准正交基底。

下面讨论一个向量乘以 i 是怎么回事。在复空间中，变换 $S=iI$是个线性变换，那么在实空间中 $v$到 $iv$的变换是否也是线性变换？注意到原来复空间的任何线性变换在实空间下也是线性变换，所以 $S$也不例外。那么它在实空间意义下对应什么矩阵呢？
 取上面的标准正交基底 $e_1,ie_1,e_2,ie_2,\dots,e_n,ie_n$，显然它在这组基底下的矩阵为
 $\begin{pmatrix}0&-1&&0&0\\ 1&0&&0&0\\  &&\ddots&&\\ 0&0&&0&-1\\  0&0&&1&0\end{pmatrix}$
 即主对角线上是 2×2 阶矩阵块
 $\begin{pmatrix}0&-1\\ 1&0\end{pmatrix}$
 其余位置都为零。

接下来讨论一般的线性变换。在复空间中每个线性变换 $T$都对应实空间的一个线性变换，那么反过来，是否一个实空间的线性变换在复空间中也是线性变换呢？
 $T(u+v)=Tu+Tv$这一条是两种空间都共同满足的，但关键是 $T(kv)=kTv$这个性质，对于实空间，我们只要求 $k$为实数，但对于复空间，我们还要求 $k$可以取复数。那么就必须有 $T(au+biu)=aTu+biTu$，换句话说，就是 $T$与上面定义的 $S$可交换：$TSv=STv$，只有这样的  $T$才有资格作为复空间的线性变换，并且这是个充要条件。
 分析一下 $\mathcal L(V)$的维数，当 $V$是复空间时，$\mathcal L(V)$的维数是 $n^2$，把 $\mathcal L(V)$看成相应的实数空间，维数也不过是 $2n^2$，但如果把 $V$看成相应的实空间，那么 $\mathcal L(V)$的维数是 $4n^2$，可见，复空间上的线性变换比相应的实空间的线性变换少了一半，原因就是复空间的特性对线性变换会有更多的限制。
 利用矩阵的分块乘法规则可以证明，实空间上的 $2n\times 2n$ 阶矩阵 $A$，如果与 $S$ 对应的矩阵可交换，那么 $A$ 有以下形式：
 $\begin{pmatrix}a&-b&&c&-d\\  b&a&&d&c\\ &&\ddots&&\\  e&-f&&g&-h\\ f&e&&h&g\end{pmatrix}$
 显然，它对应复空间上的矩阵
 $\begin{pmatrix}a+bi&\dots&c+di\\ \vdots&\ddots&\vdots\\ e+fi&\dots&g+hi\end{pmatrix}$
 而且，分析这种矩阵的自由度，与上面分析的复空间变换的维数恰好吻合。

下面利用实空间的理论，在复空间衍生的实空间中证明命题 7.2。首先证明一条引理：

**引理1：** 如果 $V$ 是实的内积空间，$T\in\mathcal L(V)$，那么 $T$ 满足
 $\forall v\in V,\langle Tv,v\rangle=0$
 当且仅当 $T$ 是斜自伴的，即 $T^\star=-T$。
 **证明：** 如果 $T^\star=-T$，那么有 $\langle Tv,v\rangle=\langle  v,T^\star v\rangle=-\langle v,Tv\rangle=-\langle Tv,v\rangle$，因此 $\langle  Tv,v\rangle=0$。
 反过来，如果 $\forall v\in V,\langle Tv,v\rangle=0$，那么
 $\begin{aligned}\langle T(v+w),v+w\rangle=&\langle  Tv,v\rangle+\langle Tv,w\rangle+\langle Tw,v\rangle+\langle  Tw,w\rangle\\ =&0\end{aligned}$
 因此
 $\begin{aligned}\langle Tv,w\rangle+\langle Tw,v\rangle=&\langle  Tv,w\rangle+\langle w,T^{\star}v\rangle\\ =&\langle Tv,w\rangle+\langle  T^{\star}v,w\rangle\\ =&\langle (T+T^{\star})v,w\rangle\\ =&0\end{aligned}$
 取 $w=(T+T^{\star})v$即可得证。

接下来把命题7.2 翻译成实空间中的命题并证明之。

**命题2**：$V$是实内积空间，$T$ 与 $S$ 都是 $V$上的斜自伴算子，且 $S$ 可逆，$T$ 与 $S$ 可交换，并且任意向量 $v$，$\langle  Tv,Sv\rangle=0$，那么 $T=0$。
 证明：由 $\langle Tv,Sv\rangle=-\langle STv,v\rangle=0$，可知 $ST$ 也是斜自伴的。那么
 $-TS=(TS)^{\star}=S^{\star}T^{\star}=ST=TS$，因此 $TS=0$。又 $S$ 可逆，故 $T=0$。证毕。
 （P.S: 如果没有 $S$ 可逆的条件，那么 $\mathrm{null}\,T\supset\mathrm{range}\,S$。）

原来一个复向量空间上的线性变换相当于暗中假定了这么多的条件！难怪复空间中的算子理论有那么好的性质。

------

2015/10/28 补充：

我们可以完善引理1，使得通过这个引理可以更直接地证明书上的命题7.2：

**引理1'：** 设 $V$是实数域或复数域上的内积空间，$T\in\mathcal L(V)$。记$i\mathbb R$为所有纯虚数和$0$构成的集合，那么，$T$ 满足
 $\forall v\in V,\langle Tv,v\rangle\in i\mathbb R$
 当且仅当 $T$是斜自伴的，即 $T^{\star}=-T$。
 **证明：** 如果 $T^{\star}=-T$，那么有 $\langle Tv,v\rangle=\langle  v,T^{\star}v\rangle=-\langle v,Tv\rangle=-\overline{\langle Tv,v\rangle}$，因此  $\langle Tv,v\rangle\in i\mathbb R$。
 反过来，如果 $\forall v\in V,\langle Tv,v\rangle\in i\mathbb R$，那么
 $\begin{aligned}\langle T(v+w),v+w\rangle=&\langle  Tv,v\rangle+\langle Tv,w\rangle+\langle Tw,v\rangle+\langle  Tw,w\rangle\\ \in& i\mathbb R\end{aligned}$
 因此
 $\begin{aligned}\Re\langle Tv,w\rangle+\langle  Tw,v\rangle=&\Re(\langle Tv,w\rangle+\langle w,T^{\star}v\rangle)\\  =&\Re(\langle Tv,w\rangle+\langle T^{\star}v,w\rangle)\\ =&\Re\langle  (T+T^{\star})v,w\rangle\\ =&0\end{aligned}$
 取 $w=(T+T^\star)v$ 即可得 $\forall v, \|(T+T^\star)v\|^2=0$，因此 $T=-T^\star$。

这样，在复数域空间中，如果$T$满足$\forall v\in V,\langle Tv,v\rangle  =0$，那么$T$和$iT$就同时满足引理1’的条件，所以同时有$T=-T^\star$和$iT=-(iT)^\star=iT^\star$，因此$T=0$。