---
title: SLAM综述
date: 2022-03-02 11:14:56
tags:
  - SLAM
categories:
  - Research
mathjax: true
---

> 关于SLAM（simultaneous localization and mapping）问题一个很粗糙的总结。

对于SLAM的发展历程，Leonard和Reid大佬将SLAM到目前为止的发展过程总结为三个阶段：

1. classical age（1986-2004）：早期阶段，SLAM问题的定义、基于概率框架的建模和求解方法；
2. algorithm-analysis age（2004-2015）：深入研究SLAM问题的一些性质，比如稀疏性、收敛性、一致性等，更多样、更高效的算法也被相继提出；
3. robust-perception age（2015-）：开始考虑算法的鲁棒性、可扩展性、资源约束下的高效算法、高层语义认知任务导向等；

SALM系统主要包括前端和后端两部分，前端负责从传感器数据中提取特征、data association（如特征提取、回环检测）等，后端负责最大后验估计、滤波等；

![SLAM](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2022/slam-1.png)

根据**感知手段**可以分为几类：

- 测距测角：传感器比如毫米波雷达、声纳等；
- 视觉相机：各种相机，比如RGB-D相机等；需要从图像中提取特征点，例如SIFT等；纯视觉导航有专门的研究方向，即VO（Visual Odometry），VO+全局地图优化（例如回环检测）=visual SLAM；
- 激光雷达；
- 惯性导航：IMU，配合视觉传感器可以实现VIO；

根据**地图结构**可以分为几类（地图结构与感知手段紧密相关）：

- 基于landmarkd的，传感器通常是测距测角的，或者视觉中提取特征点；
- 栅格地图（2D）、点云地图（3D），主要是激光雷达；
- 基于边/表面的几何地图；

根据**求解方法**可以分为几类：

- 基于贝叶斯框架递归滤波的：如EKF、PF、Information     Filter等；
- 基于优化的：一种是光束平差法（用于视觉，实际上是最小二乘）；另一种是图优化（graph SLAM，实际上就是概率图模型，似乎是现在的主流）；
- Set Membership 类的方法、以及定性方法等；

要考虑的科学问题包括：

- 地图构建和机器人位姿估计，因此实际上是参数估计/求解问题；
- 多传感器数据融合，以及与已有地图信息的融合；
- 回环检测，纠正累积误差；
- 降低复杂度，比如每次只更新相关的部分地图和状态，而不是所有参数一起更新；
- 数据关联；

![SLAM](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2022/slam-2.jpg)

除了基本的SLAM问题，还有一些发展方向：

- Active SLAM：同时考虑机器人的运动控制，与exploration-exploitation问题相关；
- 多机SLAM，相比于单机需要考虑的问题：
  - 多机协作的架构：集中式（online or offline？）、分布式；
  - 相邻机器之间的数据交互，防止数据伦理问题，即一个数据在多个机器上重复使用多次；
  - 每个机器的地图更新方法；
  - 数据关联问题；
  - 通信负载、数据压缩、地图结构优化等；



## Reference

1. Dissanayake, MWM Gamini, et al. "A solution to the simultaneous localization and map building (SLAM) problem." *IEEE Transactions on robotics and automation* 17.3 (2001): 229-241.
2. Durrant-Whyte, Hugh, and Tim Bailey. "Simultaneous localization and mapping: part I." *IEEE robotics & automation magazine* 13.2 (2006): 99-110.
3. Bailey, Tim, and Hugh Durrant-Whyte. "Simultaneous localization and mapping (SLAM): Part II." *IEEE robotics & automation magazine* 13.3 (2006): 108-117.
4. Bresson, Guillaume, et al. "Simultaneous localization and mapping: A survey of current trends in autonomous driving." *IEEE Transactions on Intelligent Vehicles* 2.3 (2017): 194-220.
5. Taketomi, Takafumi, Hideaki Uchiyama, and Sei Ikeda. "Visual SLAM algorithms: A survey from 2010 to 2016." *IPSJ Transactions on Computer Vision and Applications* 9.1 (2017): 1-11.
6. Cadena, Cesar, et al. "Past, present, and future of simultaneous localization and mapping: Toward the robust-perception age." *IEEE Transactions on robotics* 32.6 (2016): 1309-1332.
