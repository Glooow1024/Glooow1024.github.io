---
title: 凸优化笔记24：ADMM
date: 2020-05-20 23:22:22
tags:
  - ADMM
  - parallel
categories:
  - Convex Optimization
mathjax: true
---

上一节讲了对偶问题上的 DR-splitting 就等价于原问题的 ADMM，这一节在详细的讲一下 ADMM 及其变种。

<!--more-->

## 1. 标准 ADMM 形式

首先还是给出 ADMM 要求解的问题的格式，也就是约束存在耦合：
$$
\begin{align}
\min_{x,z} \quad& f(x)+g(z) \\
\text{s.t.} \quad& Ax+Bz=b
\end{align}
$$
这个问题的增广拉格朗日函数为
$$
L_{\beta}(\mathbf{x}, \mathbf{z}, \mathbf{w})=f(\mathbf{x})+g(\mathbf{z})-\mathbf{w}^{\top}(\mathbf{A} \mathbf{x}+\mathbf{B z}-\mathbf{b})+\frac{\beta}{2}\|\mathbf{A} \mathbf{x}+\mathbf{B z}-\mathbf{b}\|_{2}^{2}
$$
ADMM 的迭代方程为
$$
\begin{array}{l}
\mathbf{x}^{k+1}=\operatorname{argmin}_{\mathbf{x}} L_{\beta}\left(\mathbf{x}, \mathbf{z}^{\mathbf{k}}, \mathbf{w}^{k}\right) \\
\mathbf{z}^{k+1}=\operatorname{argmin}_{\mathbf{z}} L_{\beta}\left(\mathbf{x}^{k+1}, \mathbf{z}, \mathbf{w}^{k}\right) \\
\mathbf{w}^{k+1}=\mathbf{w}^{k}-\beta\left(\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}^{k+1}-\mathbf{b}\right)
\end{array}
$$
这实际上就是把 ALM 中关于 $x,z$ 的联合优化给分开了，分别进行优化。如果取 $\mathbf{y}^{k}=\mathbf{w}^{k}/\beta$，就可以转化为
$$
\begin{array}{l}
\mathbf{x}^{k+1}=\operatorname{argmin}_{\mathbf{x}} f(\mathbf{x})+g\left(\mathbf{z}^{k}\right)+\frac{\beta}{2}\left\|\mathbf{A} \mathbf{x}+\mathbf{B} \mathbf{z}^{k}-\mathbf{b}-\mathbf{y}^{k}\right\|_{2}^{2} \\
\mathbf{z}^{k+1}=\operatorname{argmin}_{\mathbf{z}} f\left(\mathbf{x}^{k+1}\right)+g(\mathbf{z})+\frac{\beta}{2}\left\|\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}-\mathbf{b}-\mathbf{y}^{k}\right\|_{2}^{2} \\
\mathbf{y}^{k+1}=\mathbf{y}^{k}-\left(\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}^{k+1}-\mathbf{b}\right)
\end{array}
$$
最后一步也可以加一个步长系数
$$
\mathbf{y}^{k+1}=\mathbf{y}^{k}-\gamma\left(\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}^{k+1}-\mathbf{b}\right)
$$

## 2. 收敛性分析

假如 $x^\star,z^\star,y^\star$ 是该问题的最优解，那么对拉格朗日函数求导可以得到 KKT 条件
$$
\begin{array}{ll}
(\text {primal feasibility}) & \mathbf{A x}^{\star}+\mathbf{B z}^{\star}=\mathbf{b} \\
(\text {dual feasibility } I) & 0 \in \partial f\left(\mathbf{x}^{\star}\right)+\mathbf{A}^{T} \mathbf{y}^{\star} \\
(\text {dual feasibility } I I) & 0 \in \partial g\left(\mathbf{z}^{\star}\right)+\mathbf{B}^{T} \mathbf{y}^{\star}
\end{array}
$$
由于 $\mathbf{z}^{k+1}=\operatorname{argmin}_{\mathbf{z}} g(\mathbf{z})+\frac{\beta}{2}\left\|\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}-\mathbf{b}-\mathbf{y}^{k}\right\|_{2}^{2}$，求导就可以得到
$$
\Rightarrow 0 \in \partial g\left(\mathbf{z}^{k+1}\right)+\mathbf{B}^{T}\left(\mathbf{A} \mathbf{x}^{k+1}+\mathbf{B} \mathbf{z}^{k+1}-\mathbf{b}-\mathbf{y}^{k}\right)=\partial g\left(\mathbf{z}^{k+1}\right)+\mathbf{B}^{T} \mathbf{y}^{k+1}
$$
也就是说对偶可行性 $II$ 在每次迭代过程中都能满足，但是对偶可行性 $I$ 则不能满足，因为有
$$
0 \in \partial f\left(\mathbf{x}^{k+1}\right)+\mathbf{A}^{T}\left(\mathbf{y}^{k+1}+\mathbf{B}\left(\mathbf{z}^{k}-\mathbf{z}^{k+1}\right)\right)
$$
当 $k\to\infty$ 的时候 $I$ 还是可以渐近逼近的。

**收敛性**：如果假设 $f,g$ 是闭凸函数，并且 KKT 条件的解存在，那么 $\mathbf{A} \mathbf{x}^{k}+\mathbf{B z}^{k} \rightarrow \mathbf{b}$，$f\left(\mathbf{x}^{k}\right)+g\left(\mathbf{z}^{k}\right) \rightarrow p^{*}$，$\mathbf{y}^{k}$ 收敛。并且如果 $(x^k,y^k)$ 有界，他们也收敛。

**收敛速度**：ADMM 算法的收敛速度没有一个 general 的分析和结论。在不同的假设条件下有不同的结论。

- 如果每步更新都有关于 $x^k,y^k,z^k$ 的准确解，并且 $f$ 光滑，$\nabla f$ 利普希茨连续，那么收敛速度为 $O(1/k),O(1/k^2)$（应该是针对不同情况可能有不同速度，课上也没怎么讲，了解一下就够了）
- ......

## 3. ADMM 变种

在 ADMM 的标准形式里比较关键的实际上就是要求一个如下形式的子问题（极小化问题）
$$
\min _{\mathbf{x}} f(\mathbf{x})+\frac{\beta}{2}\|\mathbf{A} \mathbf{x}-\mathbf{v}\|_{2}^{2}
$$
其中 $\mathbf{v}=\mathbf{b}-\mathbf{B} \mathbf{z}^{k}+\mathbf{y}^{k}$。这个问题对于不同的 $f$ 求解复杂度也不一样，而且有的时候并不能得到准确的解，只能近似。实际上这也是一个优化问题，可以采用的方法有

1. 迭代方法，比如 CG，L-BFGS；
2. 如果 $f(x)=1/2 \|Cx-d\|^2$，那么子问题就是求解方程 $(C^TC+\beta A^TA)x^{k+1}=\cdots$，由于 $C$ 是固定的参数，因此可以在一开始做一次 Cholesky 分解或者 $LDL^T$ 分解，之后求解就很简单了。另外如果 $(C^TC+\beta A^TA)$ 的结构是简单矩阵 + 低秩矩阵，就可以用 Woodbury 公式矩阵求逆；
3. 单次梯度下降法 $\mathbf{x}^{k+1}=\mathbf{x}^{k}-c^{k}\left(\nabla f\left(\mathbf{x}^{k}\right)+\beta \mathbf{A}^{T}\left(\mathbf{A} \mathbf{x}+\mathbf{B} \mathbf{z}^{k}-\mathbf{b}-\mathbf{y}^{k}\right)\right)$
4. 如果 $f$ 非光滑，也可以把上面的梯度下降换成 proximal 梯度下降；
5. 可以在后面加一个正则项

$$
\mathbf{x}^{k+1}=\operatorname{argmin} f(\mathbf{x})+\frac{\beta}{2}\left\|\mathbf{A} \mathbf{x}+\mathbf{B y}^{k}-\mathbf{b}-\mathbf{z}^{k}\right\|_{2}^{2}+\frac{\beta}{2}\left(\mathbf{x}-\mathbf{x}^{k}\right)^{T}\left(\mathbf{D}-\mathbf{A}^{T} \mathbf{A}\right)\left(\mathbf{x}-\mathbf{x}^{k}\right)
$$

这个时候优化问题就变成了 $\min f(x)+(\beta/2)(x-x^k)^TD(x-x^k)$，如果取一个简单的 $D$ 比如 $D=I$，那么问题就可能得到简化。

## 4. 分布式 ADMM

回想我们之前在计算近似点以及近似点梯度下降的时候，如果函数 $f,g$ 有特殊结构是不是可以分布式并行计算，而 ADMM 的子问题实际上跟近似点算子很像，所以如果有一定的特殊结构也可以并行处理。

首先回忆一下 ADMM 子问题的形式
$$
\min _{\mathbf{x}} f(\mathbf{x})+\frac{\beta}{2}\|\mathbf{A} \mathbf{x}-\mathbf{v}\|_{2}^{2}
$$
现在这个优化变量 $\mathbf{x}$ 是一个向量，我们的思想就是 $\mathbf{x}$ 分成多个子块 $x_1,...,x_n$，如果函数有特殊的形式，就能把上面的问题解耦成多个子项的求和，然后针对 $x_1,...,x_n$ 就能并行求解了。下面看几种特殊形式。

### 4.1 Distributed ADMM Ⅰ

函数 $f$ 需要是可分的
$$
f(\mathbf{x})=f_{1}\left(\mathbf{x}_{1}\right)+f_{2}\left(\mathbf{x}_{2}\right)+\cdots+f_{N}\left(\mathbf{x}_{N}\right)
$$
约束条件 $A\mathbf{x}+B\mathbf{z}=\mathbf{b}$ 也需要是可分的
$$
\mathbf{A}=\left[\begin{array}{cccc}\mathbf{A}_{1} & & & \mathbf{0} \\& \mathbf{A}_{2} & & \\& & \ddots & \\\mathbf{0} & & & \mathbf{A}_{N}\end{array}\right]
$$
如果满足上面的两个性质，那么原本的更新过程
$$
\mathbf{x}^{k+1} \leftarrow \min f(\mathbf{x})+\frac{\beta}{2}\left\|\mathbf{A} \mathbf{x}+\mathbf{B y}^{k}-\mathbf{b}-\mathbf{z}^{k}\right\|_{2}^{2}
$$
就可以分成并行的 $N$ 个优化问题
$$
\begin{array}{c}\mathbf{x}_{1}^{k+1} \leftarrow \min f_{1}\left(\mathbf{x}_{1}\right)+\frac{\beta}{2}\left\|\mathbf{A}_{1} \mathbf{x}_{1}+\left(\mathbf{B} \mathbf{y}^{k}-\mathbf{b}-\mathbf{z}^{k}\right)_{1}\right\|_{2}^{2} \\\vdots \\\mathbf{x}_{N}^{k+1} \leftarrow \min f_{N}\left(\mathbf{x}_{N}\right)+\frac{\beta}{2}\left\|\mathbf{A}_{N} \mathbf{x}_{N}+\left(\mathbf{B} \mathbf{y}^{k}-\mathbf{b}-\mathbf{z}^{k}\right)_{N}\right\|_{2}^{2}\end{array}
$$
**例子 1**(consensus)：假如我们的 $f$ 并不像上面那样可分，而是 $\min\sum_{i=1}^N f_i(\mathbf{x})$，注意上面要求 $f_i,f_j$ 的自变量分别是 $x_i,x_j$，而这里的 $f_i,f_j$ 的自变量都是 $\mathbf{x}$。可以怎么办呢？引入 $\mathbf{x}$ 的 $N$ 个 copies，把优化问题写成
$$
\begin{align}\min_{\{\mathbf{x}_i\},\mathbf{z}} \quad& \sum_i f_i(\mathbf{x}_i) \\\text{s.t.} \quad& \mathbf{x}_i-\mathbf{z}=0,\forall i\end{align}
$$
**例子 2**(exchange)：优化问题的形式为
$$
\begin{align}\min_{\{\mathbf{x}_i\},\mathbf{z}} \quad& \sum_i f_i(\mathbf{x}_i) \\\text{s.t.} \quad& \sum_i\mathbf{x}_i=0\end{align}
$$
这个问题的满足 $f$ 可分了，但是却不满足上面要求的 $A$ 的形式。可以怎么做呢？再次引入变量
$$
\begin{align}\min_{\{\mathbf{x}_i\},\mathbf{z}} \quad& \sum_i f_i(\mathbf{x}_i) \\\text{s.t.} \quad& \mathbf{x}_i-\mathbf{x}_i'=0,\forall i \\\quad& \sum_i\mathbf{x}_i'=0\end{align}
$$
这个时候 $\mathbf{x}_i$ 可以并行计算了，但是 $\mathbf{x}_i'$ 还需要处理
$$
(\mathbf{x}_i')^{k+1}=\arg\min_{\{\mathbf{x}_i'\}} \sum_i \frac{\beta}{2}\|\mathbf{x}_i^{k+1}-\mathbf{x}_i' + \mathbf{y}_i^k/\beta\| \\\text{s.t.} \sum_i \mathbf{x}_i'=0
$$
这个问题可以得到闭式解，代入关于 $\mathbf{x}_i$ 的迭代方程里就可以得到
$$
\begin{aligned}\mathbf{x}_{i}^{k+1} &=\underset{\mathbf{x}_{i}}{\operatorname{argmin}} f_{i}\left(\mathbf{x}_{i}\right)+\frac{\beta}{2}\left\|\mathbf{x}_{i}-\left(\mathbf{x}_{i}^{k}-\operatorname{mean}\left\{\mathbf{x}_{i}^{k}\right\}-\mathbf{u}^{k}\right)\right\|_{2}^{2} \\\mathbf{u}^{k+1} &=\mathbf{u}^{k}+\operatorname{mean}\left\{\mathbf{x}_{i}^{k+1}\right\}\end{aligned}
$$
实际上，这个 exchange 问题还是 consensus 问题的对偶形式，只需要写出来拉格朗日函数和 KKT 条件就可以了。

### 4.2 Distributed ADMM Ⅱ

前面是对 $\mathbf{x}$ 进行分解，其实我们还可以对 $\mathbf{z}$ 进行分解。对于约束 $A\mathbf{x}+\mathbf{z}=\mathbf{b}$，可以按行分解
$$
\mathbf{A}=\left[\begin{array}{c}\mathbf{A}_{1} \\\vdots \\\mathbf{A}_{L}\end{array}\right], \mathbf{z}=\left[\begin{array}{c}\mathbf{z}_{1} \\\vdots \\\mathbf{z}_{L}\end{array}\right], \mathbf{b}=\left[\begin{array}{c}\mathbf{b}_{1} \\\vdots \\\mathbf{b}_{L}\end{array}\right]
$$
这个时候假如优化函数的形式为 $\min_{\mathbf{x},\mathbf{z}}\sum_l (f_l(\mathbf{x})+g_l(\mathbf{z}_l)),\text{ s.t.}A\mathbf{x}+\mathbf{z}=\mathbf{b}$，注意到这个时候虽然关于 $\mathbf{z}$ 是可分的，但是关于 $\mathbf{x}$ 却不是，跟前面的方法类似，我们把 $\mathbf{z}$ copy 很多份，就可以转化为
$$
\begin{align}\min_{\mathbf{x},\{\mathbf{x}_l\},\mathbf{z}} \quad& \sum_l (f_l(\mathbf{x}_l)+g_l(\mathbf{z}_l)) \\\text{s.t.} \quad& A_l\mathbf{x}_l+\mathbf{z}_l=\mathbf{b}_l,\forall i \\\quad& \mathbf{x}_l-\mathbf{x}=0\end{align}
$$
ADMM 方法中第一步我们更新 $\{\mathbf{x}_l\}$ 这可以并行处理，第二步我们更新 $\mathbf{x},\mathbf{z}$，巧妙的是我们也可以把他们两个解耦合开再并行处理。

### 4.3 Distributed ADMM Ⅲ

实际上前面分别是对矩阵 $A$ 按列分解和按行分解，那也很容易想到我们可以既对列分解也对行分解。

对优化问题
$$
\begin{align}\min \quad& \sum_j f_j(\mathbf{x}_j)+\sum_i g_i(\mathbf{z}_i) \\\text{ s.t.}\quad& A\mathbf{x}+\mathbf{z}=\mathbf{b}\end{align}
$$
那么就可以分解为
$$
\mathbf{A}=\left[\begin{array}{cccc}\mathbf{A}_{11} & \mathbf{A}_{12} & \cdots & \mathbf{A}_{1 N} \\\mathbf{A}_{21} & \mathbf{A}_{22} & \cdots & \mathbf{A}_{2 N} \\& & \ldots & \\\mathbf{A}_{M 1} & \mathbf{A}_{M 2} & \cdots & \mathbf{A}_{M N}\end{array}\right], \text { also } \mathbf{b}=\left[\begin{array}{c}\mathbf{b}_{1} \\\mathbf{b}_{2} \\\vdots \\\mathbf{b}_{M}\end{array}\right]
$$
优化问题可以转化为
$$
\begin{align}\min \quad& \sum_j f_j(\mathbf{x}_j)+\sum_i g_i(\mathbf{z}_i) \\\text{ s.t.}\quad& \sum_j A_{ij}\mathbf{x}_j+\mathbf{z}_i=\mathbf{b}_i,i=1,...,M\end{align}
$$
但是注意到这个时候 $\mathbf{x}_j$ 之间还是相互耦合的，类比前面的方法，要想解耦合，我们就找一个“替身”，这次是 $\mathbf{p}_{ij}=A_{ij}\mathbf{x}_j$，那么新的问题就是
$$
\begin{align}\min \quad& \sum_j f_j(\mathbf{x}_j)+\sum_i g_i(\mathbf{z}_i) \\\text{ s.t.}\quad& \sum_j \mathbf{p}_{ij}+\mathbf{z}_i=\mathbf{b}_i,\forall i \\\quad& \mathbf{p}_{ij}=A_{ij}\mathbf{x}_j,\forall i,j\end{align}
$$
ADMM 中可以交替更新 $\{\mathbf{p}_{ij}\}$ 和 $(\{\mathbf{x}_j\},\{\mathbf{z}_i\})$。关于 $\{\mathbf{p}_{ij}\}$ 的求解有闭式解，关于 $(\{\mathbf{x}_j\},\{\mathbf{z}_i\})$ 也是可以分解为分别更新 $\{\mathbf{x}_j\},\{\mathbf{z}_i\}$，但是需要注意的是更新 $\mathbf{x}_j$ 的时候 $f_j,A_{1j}^TA_{1j},...,A_{Mj}^TA_{Mj}$ 都耦合在一起了，实际当中计算应该还是比较麻烦的。

### 4.3 Distributed ADMM Ⅳ

既然上面第三类方法中还是有耦合，那我们就可以再引入“替身变量”来解耦合，对每个 $\mathbf{x}_j$ 都 copy 出来 $\mathbf{x}_{1j},...,\mathbf{x}_{Mj}$，之后可以得到
$$
\begin{align}\min \quad& \sum_j f_j(\mathbf{x}_j)+\sum_i g_i(\mathbf{z}_i) \\\text{ s.t.}\quad& \sum_j \mathbf{p}_{ij}+\mathbf{z}_i=\mathbf{b}_i,\forall i \\\quad& \mathbf{p}_{ij}=A_{ij}\mathbf{x}_{ij},\forall i,j \\\quad& \mathbf{x}_{j}=\mathbf{x}_{ij},\forall i,j\end{align}
$$
ADMM 中可以交替更新 $(\{\mathbf{x}_j\},\{\mathbf{p}_{ij}\})$ 和 $(\{\mathbf{x}_{ij}\},\{\mathbf{z}_i\})$



