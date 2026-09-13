---
title: 【高等数值分析】数值积分和数值微分
date: 2022-01-09 21:55:54
tags:
  - 数值积分
categories:
  - 高等数值分析
mathjax: true
---

## 1. 预备理论

根据Newton-Leibniz公式有 $\int_a^x f(t)dt = F(x)-F(a)$，但是绝大部分情况很难解析求解，需要数值积分。例如**中点公式**
$$
\int_a^b f(x)dx \approx f(\frac{a+b}{2})(b-a)
$$
若 $f(x)\in C^2[a,b]$，则中点公式**截断误差**为
$$
\int_a^b f(x)dx - f(\frac{a+b}{2})(b-a) = \frac{(b-a)^2}{24} f''(\xi), \quad \xi\in(a,b)
$$
<!--more-->

## 2. 插值型求积公式

顾名思义，就是先插值再求积分。方法为给定求积节点 $x_k,k=0,1,...,n$ 和求积系数 $A_k,k=0,1,...,n$，插值型求积公式表示为
$$
\int_a^b f(x)dx \approx \sum_{k=0}^n A_k f(x_k)
$$
根据多项式插值中的理论，余项可表示为
$$
E_n(f) = \frac{1}{(n+1)!}\int_a^b f^{(n+1)}(\xi(x)) w_{n+1}(x) dx
$$
当 $f(x)\in{\mathcal P}_n$ 时都有 $E_n(f)=0$。由此可以定义代数精度，如果对所有 $p\in{\mathcal P}_m$ 有 $E_n(p)=0$，而对某个 $q\in{\mathcal P}_{m+1}$ 有 $E_n(q)\ne 0$，称求积公式具有 $m$ 次**代数精度**。

插值型求积公式主要分为两类：Newton-Cotes求积公式和Gauss型求积公式。前者等距选取插值节点，后者则未必。

### 2.1 Newton-Cotes求积公式

方法是将区间 $[a,b]$ $n$ 等分，得到 $h = \frac{b-a}{n},x_k=a+kh,k=0,...,n$，再利用Lagrange插值公式 $l_k(x)$，得到
$$
\int_a^b f(x)dx = (b-a)\sum_{k} \frac{f(x_k)}{b-a}\int_a^b l_k(x)dx = (b-a)\sum_k C_k^{(n)} f(x_k)
$$
其中 $C_k^{(n)}=\frac{1}{b-a}\int_a^b \prod_{j\ne k}\frac{x-x_j}{x_k-x_j}dx = \frac{1}{n}\int_a^b \prod_{j\ne k}\frac{t-j}{k-j}dx$ 称为 Cotes 求积系数，不仅**与被积函数无关**，**与求积区间也无关**。

该方法有如下性质：

- $\sum_{k=0}^n C_k^{(n)} = 1$（取 $f\equiv1$ 即可得证）；
- $E_n(f) = \frac{1}{(n+1)!}\int_a^b f^{(n+1)}(\xi(x)) w_{n+1}(x) dx$；
- 当 $n$ 为偶数时，代数精度为 $n+1$；当 $n$ 为奇数时，代数精度为 $n$。（直观理解是因为奇次多项式的奇对称性积分后恰好为 0）

下面是 $n$ 取不同数值的特例。

$n=1$ 时为梯形公式，代数精度为 $1$：
$$
\begin{align}
&\int_a^b f(x)dx \approx \frac{b-a}{2}[f(a)+f(b)] \\
&E_1(f) = -\frac{(b-a)^3}{12} f''(\xi), \xi\in[a,b]
\end{align}
$$
$n = 2$ 时为 Simpson 公式，代数精度为 $3$：
$$
\begin{align}
&\int_a^b f(x)dx \approx \frac{b-a}{6}[f(a)+4f(\frac{a+b}{2})+f(b)] \\
&E_1(f) = -\frac{(b-a)^5}{2880} f^{(4)}(\xi), \xi\in[a,b]
\end{align}
$$

### 2.2 Gauss型求积公式

求积公式也表示为
$$
\int_a^b f(x)dx \approx \sum_{k=0}^n A_k f(x_k)
$$
但与Newton-Cotes方法不同的是求积节点并非等距选取，而是将参数待定，解出使代数精度最高的参数 $A_k$ 和 $x_k$。共有 $2n+2$ 个参数待定，分别取 $f(x)=1,x,x^2,...,x^{2n+1}$ 列方程组，因此代数精度最高可以达到 $2n+1$。

从理论上也可以证明代数精度至多为 $2n+1$。

证明：取 $f(x)=\prod_{k=0}^n(x-x_k)^2$，那么上面等式左边一定有 $\int_a^b f(x)dx > 0$，但是等式右边有 $\sum_{k} A_k f(x_k)=0$，显然不相等。证毕。

除此之外，Gauss型求积公式还可以拓展到带权积分的情况，也即
$$
\int_a^b \rho(x)f(x)dx = \sum_{k=0}^n A_k f(x_k) + \frac{1}{(n+1)!}\int_a^b f^{(n+1)}(\xi(x)) w_{n+1}(x) dx
$$
关键问题是如何求解 $A_k,x_k$ 呢？

由于代数精度为 $2n+1$，对 $\forall f\in{\mathcal P}_{2n+1}$，截断误差应满足 $\frac{1}{(n+1)!}\int_a^b f^{(n+1)}(\xi(x)) w_{n+1}(x) dx=0$。又由于 $f^{(n+1)}\in{\mathcal P}_n$，那么只需要取 $w_{n+1}(x)$ 为 $n+1$ 阶正交多项式（权函数为 $\rho$）即可满足该条件。因此Gauss型求积公式的求解方法为：

1. 权函数为 $\rho$，求 $n+1$ 阶正交多项式；
2. 求 $n+1$ 个根 $x_k$（在逼近一章中已经证明了一定存在 $n+1$ 个不同的根）；
3. 取 $f(x)=1,x,...,x^n$ 列线性方程组求 $A_k$。

Gauss型求积公式有如下性质：

- $A_k > 0,k=0,1,...,n$（取 $f(x)=l_i(x)$ 即可证明）；
- $\sum_{k=0}^n A_k = \int_a^b \rho(x)dx$（取 $f\equiv$ 即可证明）；
- 余项 $E_n(f) = \frac{1}{(2n+2)!}f^{(2n+2)}(\eta)\int_a^b \rho(x) [w_{n+1}(x)]^2 dx$。



## 3. 数值微分

数值微分和数值积分类似，也有插值型微分公式，即先进性多项式插值，再求导数值。

还可以利用Richardson外推公式提高精度。

