---
title: 统计推断(六) Modeling
date: 2020-02-03 20:00:00
tags:
  - 熵
  - 信息容量
categories: 
  - 统计推断
mathjax: true
---

模型选择

<!--more-->

## 1. Modeling problem

- formulation

  - a set of distributions 
    $$
    \mathcal{P}=\left\{p_{\mathrm{y}}(\cdot ; x) \in \mathcal{P}^{y}: x \in \mathcal{X}\right\}
    $$

  - approximation
    $$
    \min _{q \in \mathcal{P}^{y}} \max _{x \in \mathcal{X}} D\left(p_{\mathrm{y}}(\cdot ; x) \| q(\cdot)\right)
    $$

- solution

> **Theorem**: 对任意 $q \in \mathcal{P}^{y}$ 都存在一个混合模型 $q_w(\cdot) = \sum_{x \in \mathcal{X}} w(x) p_{y}(\cdot ; x)$ 满足
> $$
> D\left(p_{y}(\cdot ; x) \| q_{w}(\cdot)\right) \leq D\left(p_{y}(\cdot ; x) \| q(\cdot)\right) \quad \text { for all } x \in \mathcal{X}
> $$
> **Proof**: 应用 Pythagoras 定理
>
> 然后很容易有
>
> $$
> \max _{x \in \mathcal{X}} \min _{q \in \mathcal{P}^{y}} D\left(p_{y}(\cdot ; x) \| q(\cdot)\right)=\max _{x \in \mathcal{X}} 0=0 \\
> $$
>
> $$
> \min _{q \in \mathcal{P}} \max _{x \in \mathcal{X}} D\left(p_{\mathrm{y}}(\cdot ; x) \| q(\cdot)\right)=\min _{q \in \mathcal{P}} \max _{w \in \mathcal{P}^{\mathcal{X}}} \sum_{x} w(x) D\left(p_{\mathrm{y}}(\cdot ; x) \| q(\cdot)\right)
> $$
>
> **Theorem** (Redundancy-Capacity Theorem): 以下等式成立，且两侧最优的 $w,q$ s是相同的
> $$
> \begin{aligned} R^{+} \triangleq \min _{q \in \mathcal{P}^{\mathcal{Y}}} \max _{w \in \mathcal{P}^{\mathcal{X}}} & \sum_{x} w(x) D\left(p_{\mathrm{y}}(\cdot ; x) \| q(\cdot)\right) \\ &=\max _{w \in \mathcal{P}} \min _{q \in \mathcal{P}} \sum_{x} w(x) D\left(p_{\mathrm{y}}(\cdot ; x) \| q(\cdot)\right) \triangleq R^{-} \end{aligned}
> $$
> **Proof**: 
>
> 1. 利用后面的 Equidistance property 证明 $R^+ \le R^-$
> 2. 根据 minimax 和 maxmini 的性质，有 $R^+ \ge R^-$
> 3. 一定有 $R^+ \ge R^-$
> 4. 证明两个不等式的取等条件是在同样的 $w,q$ 处取到

## 2. Model capacity

> 首先计算 $R^-$ 内部的 min
> $$
> \begin{aligned} & \min _{q \in \mathcal{P}^{\mathcal{Y}}} \sum_{x} w(x) D\left(p_{\mathbf{y}}(\cdot ; x) \| q(\cdot)\right) \\=& \min _{q \in \mathcal{P}^{\mathcal{Y}}} \sum_{x, y} w(x) p_{\mathbf{y}}(y ; x) \log \frac{p_{y}(y ; x)}{q(y)} \\=& \text { constant }-\max _{q \in \mathcal{P}^{\mathcal{Y}}} \sum_{y} q_{w}(y) \log q(y) \\=& \text { constant }-\max _{q \in \mathcal{P}^{\mathcal{Y}}} \mathbb{E}_{q_{w}}[\log q(y)] \end{aligned}
> $$
>
> 根据 Gibbs 不等式
> $$
> q^*(\cdot) = q_{w}(\cdot) \triangleq \sum_{x \in \mathcal{X}} w(x) p_{y}(\cdot ; x)
> $$
> 再考虑 $R^-$ 外部的 max，此时可以转化为 **Bayesian** 角度！
> $$
> \begin{aligned} R^{-} &=\max _{w \in \mathcal{P}^{\mathcal{X}}} \sum_{x} w(x) D\left(p_{y}(\cdot ; x) \| q_{w}(\cdot)\right) \\ &=\max _{w \in \mathcal{P}^{\mathcal{X}}} \sum_{x, y} w(x) p_{y}(y ; x) \log \frac{p_{y}(y ; x)}{\sum_{x^{\prime}} w\left(x^{\prime}\right) p_{y}\left(y ; x^{\prime}\right)} \\
> 
> &\overset{\text{Bayesian}}{=}\max _{p_{\mathbf{x}}} \sum_{x} p_{\mathbf{x}}(x) D\left(p_{y | \mathbf{x}}(\cdot | x) \| p_{y}(\cdot)\right) \\ &=\max _{p_{\mathbf{x}}} \sum_{x, y} p_{\mathbf{x}}(x) p_{\mathbf{y} | \mathbf{x}}(y | x) \log \frac{p_{y | x}(y | x)}{p_{\mathbf{y}}(y)} \\ &=\max _{p_{\mathbf{x}}} \sum_{x, y} p_{\mathbf{x}, \mathbf{y}}(x, y) \log \frac{p_{\mathbf{x}, \mathbf{y}}(x, y)}{p_{\mathbf{x}}(x) p_{y}(y)}=\max _{p_{\mathbf{x}}} I(x ; y)=C 
> \end{aligned}
> $$
>
> **Definition**: 对一个模型 $p_{\mathsf{y|x}}$，有
> $$
> C \triangleq \max _{p_{x}} I(x ; y)
> $$
>
> - **Model capacity**: C
>
> - **least informative prior**: $p_x^*$
>
> **Theorem**(Equidistance property): C对应的最优的 $p^*$ 和 $w^*$ 满足
> $$
> D(p_y(\cdot;x)||q^*(\cdot)) \le C \ \ \ \ \ \forall x\in\mathcal{X}
> $$
> 其中等号对于满足 $w^*(x)>0$ 的 x 成立
>
> **Proof**: 
>
> 1. $I(x,y)$ 关于 $p_x(a)\ \ \forall a$  是 concave 的
> 2. 构造拉格朗日函数 $\mathcal{L}=I(x,y) - \lambda(\sum_x p_x(x)-1)$，也关于 $p_x(a)$ concave
> 3. $\min_{p_x}I(x,y)$ 的极值点应满足 $\left.\frac{\partial I(x ; y)}{\partial p_{x}(a)}\right|_{p_{x}=p_{x}^{*}}-\lambda=0, \quad \text { for all } a \in \mathcal{X} \text { such that } p_{x}^{*}(a)>0$，或者 $\left.\frac{\partial I(x ; y)}{\partial p_{x}(a)}\right|_{p_{x}=p_{x}^{*}}-\lambda\le0, \quad \text { for all } a \in \mathcal{X} \text { such that } p_{x}^{*}(a)=0$
> 4. $\frac{\partial I(x ; y)}{\partial p_{x}(a)} = D\left(p_{y | x}(\cdot ; a) \| p_{y}\right)-\log e$ 并根据 3 中取等号的特点恰好可以得到定理中的式子

## 3. Inference with mixture models

- Formulation: 有观测 $y_-$，想要预测 $y_+$

- Solution

  - 根据前面得到的最优先验 $w^*$ 来估计 $y=[y_-,y_+]$ 的分布
    $$
    q_{\mathbf{y}}^{*}(\mathbf{y})=\sum_{x} w^{*}(x) p_{\mathbf{y}}(\mathbf{y} ; x)
    $$

  - 然后可以获得后验概率
    $$
    \begin{aligned} q_{\mathrm{y}+| \mathrm{y}_{-}}^{*}\left(\cdot | y_{-}\right) & \triangleq \frac{q_{\mathrm{y}}^{*}\left(y_{+}, y_{-}\right)}{q_{\mathrm{y}-}^{*}\left(y_{-}\right)}=\frac{\sum_{x} w^{*}(x) p_{\mathrm{y}}\left(y_{+}, y_{-} ; x\right)}{\sum_{a} w^{*}(a) p_{\mathrm{y}_{-}}\left(y_{-} ; a\right)} \\ 
    &=\sum_{x} w^{*}\left(x | y_{-}\right) p_{\mathrm{y}_{+} | y_{-}}\left(y_{+} | y_{-} ; x\right) \end{aligned}
    $$

  - 相当于是做了 soft decision，因为 ML 估计中只会取 $p_{\mathrm{y}_{+} | y_{-}}(\cdot|y_-; \hat{x}_{ML})$ 

## 4. Maximum entropy distribution

- 最大熵等价于**均匀分布**向对应的模型集合上的 **I-projection**
  $$
  D(p \| U)=\sum_{y} p(y) \log p(y)+\log |\mathcal{Y}|=\log |\mathcal{Y}|-H(p) \\
  p^{*}=\underset{p \in \mathcal{L}_{\mathrm{t}}}{\arg \max } H(p)=\underset{p \in \mathcal{L}_{\mathrm{t}}}{\arg \min } D(p \| U)
  $$
  

