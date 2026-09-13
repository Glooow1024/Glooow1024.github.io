---
title: 模糊数学笔记 5：模糊聚类
date: 2020-03-07 23:39:59
tags:
  - 模糊聚类
  - 模糊矩阵
categories:
  - Fuzzy Mathematics
mathjax: true
---

现在想要对 $n$ 个目标 $U=\{x_1,...,x_n\}$ 分类，并且每个对象都有多个指标，也即 $x_i=\{x_{i1},...,x_{im}\}$。

模糊聚类通常包括三个步骤：

1. 建立模糊矩阵
2. 建立模糊等价矩阵
3. 进行聚类

<!--more-->

## 1. 数据标准化

实际中各个指标的量纲与数量级很可能相差很大，比如高校评价指标中，可能包括科研经费、论文数量、获奖数量等，数量级差别很大，这个时候就需要先对各项数据进行标准化。常用方法有

**标准差标准化**
$$
x_{ij}'=\frac{x_{ij}-\bar{x}_j}{\sigma_j}
$$
**极差正规化**
$$
\boldsymbol{x}_{i j}^{\prime \prime}=\frac{\boldsymbol{x}_{i j}^{\prime}-\min _{1 \leq i \leq n}\left\{\boldsymbol{x}_{i j}^{\prime}\right\}}{\max _{1}\left\{\boldsymbol{x}_{i j}^{\prime}\right\}-\min _{1: i \leqslant n}\left\{\boldsymbol{x}_{i j}^{\prime}\right\}}
$$
**极差标准化**
$$
x_{i j}^{\prime}=\frac{x_{i j}-\bar{x}_{i}}{\max \left\{x_{i j}\right\}-\min \left\{x_{i j}\right\}}
$$
**最大值规格化**
$$
x_{ij}'=\frac{x_{ij}}{\max{(x_{1j},x_{2j},...,x_{nj})}}
$$

## 2. 建立模糊相似矩阵

接下来需要确定不同对象之间的相似度，相似度的确定也有几种常用度量：

### 1.1 相关系数类

![correlation](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-relation.PNG)

### 1.2 距离类

![distance](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-distance.PNG)

### 1.3 贴近度类

![nearness](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-nearness.PNG)

## 3. 聚类

获得模糊相似矩阵以后，需要首先求出传递闭包 $t(R)$，以获得模糊等价矩阵，然后根据不同的阈值 $\lambda$ 进行聚类，然后再画出动态聚类图。

***举个栗子***

![exercise](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg1.PNG)

求解过程如下

| 0. 特性指标矩阵                                              | 1. 数据标准化                                                | 2. 求模糊相似矩阵                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg2.PNG) | ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg3.PNG) | ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg4.PNG) |
| **3. 求传递闭包**                                            | **4. 根据不同阈值聚类**                                      | **5. 画动态聚类图**                                          |
| ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg5.PNG) | ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg6.PNG) | ![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg7.PNG) |

## 4. 其他问题

在实际应用过程中，还会出现一些其他问题，比如：

- *在实际应用中，如何选择适当的 $\lambda$？从而给出一个较明确的聚类。*

实际中最佳阈值的确定，可以由专家给出值；也可以应用 F-统计量确定最佳阈值。

- *如果不用传递闭包，直接对相似矩阵进行聚类会怎么样？*

直接应用模糊相似矩阵进行聚类时，可以首先确定一个阈值 $\lambda$，然后根据截集获得一系列聚类结果，由于模糊相似矩阵并不是等价矩阵，因此此时的聚类结果是不严谨的，后面需要对交集非空的集合进行合并（这里可能表述的不清楚，看下面的例子就明白了）

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg8.PNG)

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg9.PNG)

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-eg10.PNG)

- *当样本点很多时（几百万甚至上千万个像素），例如需 要对一张图片上的样本点进行聚类，该怎么办？*

可以应用**模糊C均值法(FCM)**

## 5. 模糊C均值法(FCM)

设 $x_i(i=1,2,...,n)$ 是n个样本组成的样本集合，$c$ 为预定的类别数目， $u_{ij}$ 是第 $i$ 个样本对于第 $j$ 类的隶属度函数。用隶属度函数定义的聚类损失函数可以写为
$$
\sum_{j=1}^{c} \sum_{i=1}^{n} u_{i j}^{m} d_{i j}^{2}=\sum_{j=1}^{c} \sum_{i=1}^{n} u_{i j}^{m}\left\|\mathbf{p}_{j}-\mathbf{x}_{i}\right\|^{2}
$$
其中 $m>1$，$P_j$ 是第 $j$ 类的聚类中心，后面会给出公式。通常也会假设 $\sum_j u_{ij}=1$。

下面则给出该算法的推导：为了最小化损失函数，则可以应用 Lagrange 乘子法
$$
J=\sum_{i=1}^{n} \sum_{j=1}^{c} u_{i j}^{m} d_{i j}^{2}-\sum_{i=1}^{n} \lambda_{i}\left(\left(\sum_{j=1}^{c} u_{i j}\right)-1\right)
$$
对 $\lambda_i,u_{ij}$ 求偏导等于 0，则可以得到迭代公式为
$$
u_{i j}=\frac{\left(\frac{1}{d_{i j}}\right)^{2 /(m-1)}}{\sum_{k=1}^{c}\left(\frac{1}{d_{i k}}\right)^{2 /(m-1)}} \\\mathbf{p}_{j}=\frac{\sum_{i=1}^{n} u_{i j}^{m} \mathbf{x}_{i}}{\sum_{i=1}^{n} u_{i j}^{m}}
$$
由此就可以给出 FCM 算法的框架

![FCM](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/5-fcm.PNG)