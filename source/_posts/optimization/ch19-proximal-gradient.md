---
title: 凸优化笔记19：近似点梯度下降
date: 2020-04-17 17:54:41
tags:
  - PG 算法
categories:
  - Convex Optimization
mathjax: true
---

前面讲了梯度下降法、次梯度下降法，并分析了他们的收敛性。上一节讲了近似梯度算子，我们说主要是针对非光滑问题的，这一节就要讲近似梯度算子在非光滑优化问题中的应用。先回顾一下上一节最重要的一部分内容：对于指示函数 $\delta_C$ 来说近似梯度算子得到的实际上就是向集合 $C$ 的投影。

## 1. 近似点梯度下降

这一部分考虑的问题主要是
$$
\text{minimize } f(x)=g(x)+h(x)
$$
这里面 $g$ 是全空间可导的凸函数，$\text{dom }g=R^n$，$h$ 是存在不可导部分的凸函数，并且一般需要 $h$ 的近似点计算较为简单。近似点梯度下降算法是什么呢？
$$
x_{k+1}=\text{prox}_{th}(x_k-t_k\nabla g(x_k))
$$
<!--more-->

这里跟之前的梯度下降(GD)和次梯度下降(SD)的形式都不太一样，实际上看了后面的推导会发现经过转换他们还是很像的。不过怎么理解这个式子呢？举一个例子，假如 $h$ 是集合 $C$ 的指示函数，那么这个式子实际上是先沿着 $g$ 的梯度走步长 $t_k$，然后再投影到集合 $C$ 里面，可以看下面这张图。而考虑原始优化问题，$\min f=g+h$ 本身是一个无约束优化问题，但实际上把 $h$ 用一个约束函数表示，他就是一个带约束的优化问题 $\min g(x),\text{ s.t. }x\in C$，而近似点梯度下降方法要做的事情就是先优化 $g$，然后投影到约束区域 $C$ 中，可以参考下图。

![19-proximal-gradient](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/19-proximal-gradient.PNG)

根据 $\text{prox}_{th}$ 的定义，我们把上面的式子展开可以得到
$$
\begin{aligned}
x^{+} &=\underset{u}{\operatorname{argmin}}\left(h(u)+\frac{1}{2 t}\|u-x+t \nabla g(x)\|_{2}^{2}\right) \\
&=\underset{u}{\operatorname{argmin}}\left(h(u)+g(x)+\nabla g(x)^{T}(u-x)+\frac{1}{2 t}\|u-x\|_{2}^{2}\right)
\end{aligned}
$$
可以发现括号里面的式子实际上就是在 $x$ 附近对光滑的 $g$ 进行了二阶展开，而 $x^+$ 就是对近似后函数取最小值点。再进一步地
$$
0\in t\partial h(x^+) + (x^+-x+t\nabla g(x)) \\
\Longrightarrow G_t(x):=\frac{x-x^+}{t}\in \partial h(x^+)+\nabla g(x)
$$
可以发现 $G_t(x)=\partial h(x^+)+\nabla g(x)$ 实际上就近似为函数 $f$ 的次梯度，但并不严格是，因为 $\partial f(x)=\partial h(x)+\nabla g(x)$。而此时我们也可以将 $x^+$ 写成比较简单的形式
$$
x^+ = x-tG_t(x)
$$
这跟之前的梯度下降法就统一了，并且也说明了 $G_t(x)$ 就相当于是 $f$ 的梯度。

这里还需要说明的一点是 $G_t(x)=(1/t)(x-\text{prox}_{th}(x-t\nabla g(x))$ 这是一个连续函数，这是因为近似点算子是 Lipschitz 连续的(在下面一小节中会解释说明)，又由于 $G_t(x)=0\iff x=\arg\min f(x)$，因此 $\Vert x-x^+\Vert\le \varepsilon$ 就可以作为 stopping criterion。与之成对比的是非光滑函数的次梯度下降，$\Vert x-x^+\Vert$ 就不是一个很好的 stopping criterion，因为即使 $\Vert x-x^+\Vert$ 很小，也可能离最优解比较远。

## 2. 收敛速度分析

在分析收敛速度之前，我们需要首先分析一下 $g(x)$ 和 $h(x)$ 这两部分函数的性质。

首先是 $h$，如果 $h$ 为闭的凸函数，那么 $\text{prox}_h(x)=\arg\min_u\left(h(u)+(1/2)\Vert u-x\Vert^2\right)$ 对每个 $x$ 一定存在唯一的解。并且 $u=\text{prox}_h(x) \iff x-u\in \partial h(u)$，那么我们就可以得到 **ﬁrmly nonexpansive(co-coercivite)** 性质：
$$
\left(\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right)^{T}(x-y) \geq\left\|\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right\|_{2}^{2}
$$
证明过程可以取 $u=\text{prox}_h(x),v=\text{prox}_h(y)$，然后根据 $x-u\in \partial h(u),y-v\in \partial h(v)$，再利用次梯度算子的单调性质就可以得到。之前在梯度下降法中第一次讲到 co-coercive 性质的时候也提到，他跟 Lipschitz continuous 性质实际上是等价的，因此我们也有(**nonexpansiveness**性质)
$$
\left\|\operatorname{prox}_{h}(x)-\operatorname{prox}_{h}(y)\right\|_2 \le \left\|x-y\right\|_2
$$
然后我们再来看函数 $g$ 的性质，类似前面梯度下降法中的两个重要性质：

1. **L-smooth**：$\frac{L}{2}x^Tx-g(x)$ convex
2. **m-strongly convex**：$g(x)-\frac{m}{2}x^Tx$ convex

然后就可以得到两个二次的界
$$
\frac{m t^{2}}{2}\left\|G_{t}(x)\right\|_{2}^{2} \leq g\left(x-t G_{t}(x)\right)-g(x)+t \nabla g(x)^{T} G_{t}(x) \leq \frac{L t^{2}}{2}\left\|G_{t}(x)\right\|_{2}^{2}
$$
如果取 $0< t\le 1/L$，那么就有 $Lt\le1,mt\le 1$。

结合上面对 $g$ 和 $h$ 性质的分析，就能得到下面这个**非常重要**的式子：

> $$
> f\left(x-t G_{t}(x)\right) \leq f(z)+G_{t}(x)^{T}(x-z)-\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2}-\frac{m}{2}\|x-z\|_{2}^{2} \qquad (\bigstar)
> $$
>
> **证明**：
> $$
> \begin{aligned}
> f\left(x-t G_{t}(x)\right) & \\
> \leq & g(x)-t \nabla g(x)^{T} G_{t}(x)+\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2}+h\left(x-t G_{t}(x)\right) \\
> \leq & g(z)-\nabla g(x)^{T}(z-x)-\frac{m}{2}\|z-x\|_{2}^{2}-t \nabla g(x)^{T} G_{t}(x)+\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2} \\
> &+h\left(x-t G_{t}(x)\right) \\
> \leq & g(z)-\nabla g(x)^{T}(z-x)-\frac{m}{2}\|z-x\|_{2}^{2}-t \nabla g(x)^{T} G_{t}(x)+\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2} \\
> &+h(z)-\left(G_{t}(x)-\nabla g(x)\right)^{T}\left(z-x+t G_{t}(x)\right) \\
> =& g(z)+h(z)+G_{t}(x)^{T}(x-z)-\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2}-\frac{m}{2}\|x-z\|_{2}^{2}
> \end{aligned}
> $$
> 其中第一个不等号用到了 $g(x)$ 凸函数以及 Lipschitz 连续的性质，第二个不等号用到了 $g(x)$ 凸函数的性质，第三个不等号用到了 $h(x)$ 凸函数的性质。

有了上面这个式子就可以分析收敛性了。

如果我们取 $z=x$，那么就有下面的式子，说明序列 $\{f(x_k\}$ 总是在减小的，如果 $f(x)$ 存在下界，那么 $f(x_k)$ 将趋向于这个下界。
$$
f(x^+)\le f(x)-\frac{t}{2}\Vert G_t(x)\Vert^2
$$
如果我们取 $z=x^\star$，那么就有
$$
\begin{aligned}
f\left(x^{+}\right)-f^{\star} & \leq G_{t}(x)^{T}\left(x-x^{\star}\right)-\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2}-\frac{m}{2}\left\|x-x^{\star}\right\|_{2}^{2} \\
&=\frac{1}{2 t}\left(\left\|x-x^{\star}\right\|_{2}^{2}-\left\|x-x^{\star}-t G_{t}(x)\right\|_{2}^{2}\right)-\frac{m}{2}\left\|x-x^{\star}\right\|_{2}^{2} \\
&=\frac{1}{2 t}\left((1-m t)\left\|x-x^{\star}\right\|_{2}^{2}-\left\|x^{+}-x^{\star}\right\|_{2}^{2}\right) \\
& \leq \frac{1}{2 t}\left(\left\|x-x^{\star}\right\|_{2}^{2}-\left\|x^{+}-x^{\star}\right\|_{2}^{2}\right)
\end{aligned}
$$
从这个式子就可以看出来很多有用的性质了：

1. $\left\|x^{+}-x^{\star}\right\|_{2}^{2}\le (1-m t)\left\|x-x^{\star}\right\|_{2}^{2}$，如果满足强凸性质的话，也即 $m>0$，就有 $\left\|x^{+}-x^{\star}\right\|_{2}^{2}\le c^k\left\|x-x^{\star}\right\|_{2}^{2},c=1-m/L$；
2. $\sum_i^k (f(x_i)-f^\star) \le \frac{1}{2t}\left\|x^{+}-x^{\star}\right\|_{2}^{2}$，由于 $f(x_i)$ 不增，因此 $f(x_k)-f^\star \le \frac{1}{2kt}\left\|x^{+}-x^{\star}\right\|_{2}^{2}$，因此收敛速度也是 $O(1/k)$。

注意到前面的分析是针对固定步长 $t\in(0,1/L]$ 的，如果我们想走的更远一点，下降的快一点呢？就可以用前几节提到的线搜索方法。也就是说每次选择步长 $t_k$ 的时候需要迭代 $t:=\beta t$ 来进行搜索，使得满足下面的式子
$$
g\left(x-t G_{t}(x)\right) \leq g(x)-t \nabla g(x)^{T} G_{t}(x)+\frac{t}{2}\left\|G_{t}(x)\right\|_{2}^{2}
$$
这个式子就是 Lipschitz 连续导出的二次上界，注意应用线搜索的时候，每次迭代我们都要额外计算一次 $g$ 和 $\text{prox}_{th}$，这个计算可能并不简单，因此不一定会使算法收敛更快，需要慎重考虑。另外为了保证能在有限步停止搜索 $t_k$，还需要加入最小步长的约束 $t\ge t_{\min}=\min \{\hat{t},\beta/L\}$。线搜索直观理解可以如下图所示

![19-line-search](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/19-line-search.PNG)

我们再来分析一下收敛性，跟前面固定步长很像，只需要将原来的式子中 $t$ 替换为 $t_i$，就可以得到
$$
t_{i}\left(f\left(x_{i+1}\right)-f^{\star}\right) \leq \frac{1}{2}\left(\left\|x_{i}-x^{\star}\right\|_{2}^{2}-\left\|x_{i+1}-x^{\star}\right\|_{2}^{2}\right)
$$
于是有

1. $\left\|x^{+}-x^{\star}\right\|_{2}^{2}\le (1-m t_i)\left\|x-x^{\star}\right\|_{2}^{2}\le (1-m t_{\min})\left\|x-x^{\star}\right\|_{2}^{2}$，如果满足强凸性质的话，也即 $m>0$，就有 $\left\|x^{+}-x^{\star}\right\|_{2}^{2}\le c^k\left\|x-x^{\star}\right\|_{2}^{2},c=1-mt_{\min}=\max \{1-\beta m/L,1-m\hat{t}\}$；
2. $\sum_i^k t_i(f(x_i)-f^\star) \le \frac{1}{2}\left\|x^{+}-x^{\star}\right\|_{2}^{2}$，由于 $f(x_i)$ 不增，因此 $f(x_k)-f^\star \le \frac{1}{2kt_{\min}}\left\|x^{+}-x^{\star}\right\|_{2}^{2}$，因此收敛速度也是 $O(1/k)$。

