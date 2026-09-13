---
title: 凸优化笔记 5：保凸变换
date: 2020-03-04 23:49:33
tags:
  - 保凸变换
categories:
  - Convex Optimization
mathjax: true
---

## 1. 保凸变换

前面提到过一次保凸变换，前面针对的是集合，凸集经过一定的保凸变换，映射后的集合仍然是凸集。这里复习一下

1. 任意个凸集的交集
2. 仿射变换
3. 透视变换
4. 分式线性函数

而这里要讲的是对函数经过操作以后，得到的仍然是凸函数。

<!--more-->

### 1.1 正权重求和

> 离散情况：$f=\sum\omega_i f_i,\omega_i\ge0$
>
> 连续情况：$f=\int\omega(y)f(y)dy,\omega(y)\ge0$

### 1.2 与仿射变换的复合函数

> 若 $f$ 为凸函数，则 $f(Ax+b)$ 也为凸函数。

**Remarks**：反之则不一定成立，若想成立（根据后面复合函数的原理）则仿射变换应具有一定的单调性。

### 1.3 逐元素最大值&上确界

> 若 $f_i$ 均为凸函数，则 $f(x)=\max_i\{f_1(x),...,f_n(x)\}$ 也为凸函数。

**Remarks**：这实际上可以看成是 $\text{epi} f=\bigcap \text{epi}f_i$，多个凸集的交集仍然是凸集。

> 若与仿射变换相结合，则可以得到 $f(x)=\max_i\{a_1^Tx+b_1,...,a_n^Tx+b_n\}$ 也是凸函数。

***例***：根据上述结论，可以推广得到，$n$ 个元素中最大的 $r$ 的求和也是凸函数，证明很简单。

> 若 $f(x,y)$ 关于 $x$ 是凸的，对任意 $y\in\mathcal{A}$，则 $g(x)=\sup_{y\in\mathcal{A}}f(x,y)$ 也是凸的。

**Remarks**：上述情况跟逐元素最大值是类似的，可以看成是无穷个 epigraph 的交集。

> 由上述结论，可以得到一个重要性质
>
> **Property**：若 $f$ 为凸函数，则 $f(x)=\sup\{g(x)| g\ \text{is affine},g(z)\le f(z),\forall z\}$
>
> **Remarks**：上述性质所描述的事情实际上就是 $f$ 被很多个支撑超平面(以及更靠下的平面)紧紧的围起来了。证明过程实际上就是找到每个 $x$ 对应的(支撑)超平面。
>
> ![support hyperplane](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-support.jpg)
>
> 通过上面的证明，可以得到的一个结论就是 $\forall x\in \text{int dom}f$，都存在一个 $y$ 使得 
> $$
> f(z)\ge f(x)+y^T(z-x),\forall z\in\text{dom}f
> $$
> 由此可以引出**次梯度(subgradient)**的概念
> $$
> \partial f=\{g| f(z)\ge f(x)+g^T(z-x),\forall z\in\text{dom}f \}
> $$
> 注意这里得到的是一系列梯度值的集合，这个集合有以下性质
>
> 1. $\partial f(x)\ne \varnothing, \text{if}\ x\in\text{int dom}f$
> 2. $\partial f(x)$ convex and closed
> 3. $\partial f(x)$ bounded if $x\in\text{int dom}f$

***例***：应用上述结论，可验证下面这些函数是凸的

1. 集合 $C$ 的支撑函数(support function)定义为：$S_C(x)=\sup_{y\in C}y^Tx$ (实际上可以将 $x$ 看作某个支撑超平面的法向量)
2. 到集合 $C$ 的最远距离：$f(x)=\sup_{y\in C}\Vert x-y\Vert$ (距离关于 $x,y$ 都是凸的)
3. 矩阵范数：$\Vert X\Vert_{a,b}=\sup\left\{\Vert Xv\Vert_a / \Vert v\Vert_b\right\}$ (证明过程用到了范数的等价定义，可参考 Boyd 的书)

### 1.4 下确界

前面提到了逐元素上确界，实际上就是 epigraph 的交集，而取下确界呢？是类似的，只不过对 $f(x,y)$ 的要求更严了

> 若 $f(x,y)$ 关于 $(x,y)$ 是凸的，**$C$ 是一个凸集**，则 $g(x)=\inf_{y\in C}f(x,y)$ 是凸的。

上述性质可应用下确性质定义来证明，也可以从 epigraph 角度来理解：$\forall(x,t)\in\text{epi }g$，都有 $(x,y,t)\in \text{epi }f,\text{ for some }y\in C$，所以 $\text{epi }g$ 实际上可以看作 $\text{epi }f$ 向低维空间中的一个投影，也是一个仿射变换/线性变换，因此 $\text{epi }g$ 也是凸的。

**Remarks**：注意上面还要求 $C$ 是一个凸集，因为凸函数要求其定义域也为凸集。

***例***：到集合 $C$ 最近距离：$\text{dist}(x,S)=\inf_{y\in S}\Vert x-y\Vert$ 是凸的，如果 $S$ 是凸的。

### 1.5 复合函数

两个凸函数的复合函数不一定是凸的，比如 $f(x)=-x,g(x)=x^2$，那么 $f(g(x))=-x^3$ 非凸

> #### 1. 标量复合函数
>
> 有函数 $g:R^n\to R,\ h:R\to R$，对于复合函数 $f(x)= h(g(x))$ 
>
> 1. $f(x)$ 为凸函数，若 $g$ convex，$h$ convex，$h$ **单调不减**
> 2. $f(x)$ 为凸函数，若 $g$ concave，$h$ convex，$h$ **单调增**
>
> #### 2. 向量复合函数
>
> 有函数 $g:R^n\to R^k,\ h:R^k\to R$，对于复合函数 $f(x)= h(g(x))=h(g_1(x),...,g_n(x))$ 
>
> 1. $f(x)$ 为凸函数，若 $g_i$ convex，$h$ convex，$h$ **关于每个元素**都单调不减
> 2. $f(x)$ 为凸函数，若 $g_i$ concave，$h$ convex，$h$ **关于每个元素**都单调增

证明：标量函数 $f^{\prime \prime}(x)=h^{\prime \prime}(g(x)) g^{\prime}(x)^{2}+h^{\prime}(g(x)) g^{\prime \prime}(x)$

向量复合函数 $f^{\prime \prime}(x)=g^{\prime}(x)^{T} \nabla^{2} h(g(x)) g^{\prime}(x)+\nabla h(g(x))^{T} g^{\prime \prime}(x)$

**Remarks**：

1. 回到刚开始提到仿射变换与凸函数的复合 $f(Ax+b)$ 是凸的，但是 $Affine(f(x))$ 就不一定了，若是凸函数则需要仿射变换具有一定的单调性

***例***：常见的例子有

1. $\exp g(x)$ 是凸的，如果 $g(x)$ 是凸的
2. $1/g(x)$ 是凸的，如果 $g(x)$ 是凸的且为正值
3. $\sum\log g_i(x)$ 为凹的，如果 $g_i$ 是凹的且为正值
4. $\log\sum\exp g_i(x)$ 为凸的，如果 $g_i$ 为凸的

### 1.6 透射变换

函数 $f:R^n\to R$ 的透射变换 $g:R^n\times R\to R$ 定义为
$$
g(x,t)=tf(x/t),\text{dom }g=\{(x,t)|x/t \in\text{dom }f,t>0 \}
$$

> 若 $f$ 是凸的，则 $g$ 是凸的。

**Remarks**：上述变换在一些问题中应该能够对应一些物理意义，不过我暂时还没想起来。证明也可以用 epigraph 来证明。

**例**：对负熵 $f(x)=-\log x$，相对熵为 $g(x,t)=t\log t-t\log x$

类似的，对向量函数 $KL(u,v)=-\sum(u_i\log(u_i/v_i)-u_i+v_i)$ 也是凸的，这实际上就就是 KL-divergence。



