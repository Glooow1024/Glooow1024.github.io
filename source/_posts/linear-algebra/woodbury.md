---
title: 转载 | Woodbury 恒等式
date: 2021-07-09 09:13:34
tags:
  - Woodbury 恒等式
  - 转载
categories: 
  - Linear Algebra
mathjax: true
---

> 版权声明：本文为CSDN博主「Anusat」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/Louis___Zhang/article/details/103483743

在数学（特别是线性代数）中，Woodbury矩阵恒等式是以Max A.Woodbury命名的，它 可以通过对原矩阵的逆进行秩k校正来计算某个矩阵的秩k校正的逆。这个公式的另一个名字是矩阵逆引理，谢尔曼-莫里森-伍德伯里（Sherman–Morrison–Woodbury formula）公式或只是伍德伯里公式。然而，在伍德伯里发现之前，这一等式出现在其他文献中。

## 1. 伍德伯里矩阵恒等式

$$
\left(A+UCV\right)^{-1}=A^{-1}-A^{-1}U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}
$$

其中$A$、$U$、 $C$ 和 $V$ 都表示适形尺寸的矩阵。具体来说， $A$ 的大小为 $n \times n$， $U$ 为 $n \times k$， $C$ 为 $k \times k$ $V$ 为 $k \times n$。

## 2. 扩展

不失一般性，可用单位矩阵替换矩阵 $A$ 和 $C$：

$$
\left(I+UV\right)^{-1}=I-U\left(I+VU\right)^{-1}V
$$
这里 $U=A^{-1}X, V = C Y$。

这个等式本身可以看作是两个简单等式的组合，即等式
$$
(I+P)^{-1}=I-(I+P)^{-1}P=I-P(I+P)^{-1}
$$
和所谓的 push-through 等式
$$
(I+UV)^{-1}U=U(I+VU)^{-1}
$$


的结合。

## 3. 特殊情况

当 $V, U$ 是向量时，伍德伯里恒等式退化为谢尔曼-莫里森公式，在标量情况下，它（简化版）只是：
$$
{\frac {1}{1+uv}}=1-{\frac {uv}{1+uv}}
$$
如果 $p = q$ 和 $U=V=I_p$ 是单位矩阵，那么
$$
\begin{aligned}
\left({A}+{B}\right)^{-1} &= A^{-1}-A^{-1}(B^{-1}+A^{-1})^{-1}A^{-1} \\
&= {A}^{-1}-{A}^{-1}\left({I}+{B}{A}^{-1}\right)^{-1}{B}{A}^{-1}. 
\end{aligned}
$$
继续合并上述方程最右边的项，就可以得到一下恒等式：
$$
\left({A}+{B}\right)^{-1}={A}^{-1}-\left({A}+{A}{B}^{-1}{A}\right)^{-1}
$$
此等式的另一个有用的形式是：
$$
\left({A}-{B}\right)^{-1}={A}^{-1}+{A}^{-1}{B}\left({A}-{B}\right)^{-1}
$$
它有一个递归结构：
$$
\left({A}-{B}\right)^{-1}=\sum _{k=0}^{\infty }\left({A}^{-1}{B}\right)^{k}{A}^{-1}
$$
这种形式可用于微扰展开式，其中$B$ 是 $A$ 的微扰。

## 4. 推广

二项式逆定理（Binomial Inverse Theorem）
如果 $A,U,B,V$ 分别是 $p\times p,p\times q, q\times q, q\times p$ 的矩阵，那么:
$$
\left(A+UBV\right)^{-1}=A^{-1}-A^{-1}UB\left(B+BVA^{-1}UB\right)^{-1}BVA^{-1}
$$
前提是 $A$ 和 $B+BVA^{-1}UB$ 是非奇异的。后者的非奇异性要求 $B^{-1}$ 存在，因为它等于 $B(I+VA＝1ub)$ ，并且后者的秩不能超过 $B$ 的秩。由于 $B$ 是可逆的，所以在右手边的附加量逆的两边的两个 $B$ 项可以被 $(B^{-1})^{-1}$ 替换，从而得到原始的Woodbury恒等式:
$$
(A+UBV)^{-1}=A^{-1}-A^{-1}U(I+BVA^{-1}U)^{-1}BVA^{-1}
$$
在某些情况下，$A$ 是有可能是奇异的。

## 5. 延伸

公式可以通过检查  $A+UCV$ 乘以伍德伯里恒等式右侧的所谓逆得到恒等式矩阵来证明：
$$
\begin{aligned}
& \left(A+UCV\right)\left[A^{-1}-A^{-1}U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}\right] \\
=& \left\{I-U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}\right\}+\left\{UCVA^{-1}-UCVA^{-1}U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}\right\} \\
=& \left\{I+UCVA^{-1}\right\}-\left\{U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}+UCVA^{-1}U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}\right\} \\
=& UCVA^{-1}-\left(U+UCVA^{-1}U\right)\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1} \\
=& UCVA^{-1}-UC \left( C^{-1} + VA^{-1}U \right) \left(C^{-1}+VA^{-1}U\right)^{-1} VA^{-1} + UCVA^{-1}-UCVA^{-1} \left({A}+{B}\right)^{-1} \\
=& A^{-1}-A^{-1}(B^{-1}+A^{-1})^{-1}A^{-1} \\
=& {A}^{-1}-{A}^{-1}\left({I}+{B}{A}^{-1}\right)^{-1}{B}{A}^{-1}.
\end{aligned}
$$


## 参考文献

1. [https://en.wikipedia.org/wiki/Woodbury_matrix_identity](https://en.wikipedia.org/wiki/Woodbury_matrix_identit)
