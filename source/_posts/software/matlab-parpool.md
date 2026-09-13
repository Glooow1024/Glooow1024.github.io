---
title: MATLAB R2016a 无法启动并行池
date: 2020-04-08 11:49:35
tags:
  - Matlab
categories:
  - Software
---

最近在用 MATLAB 跑仿真，但是不知怎么回事，之前并行计算 parfor 用的好好的，昨天突然就不能用了，一直报错无法启动并行池，报错原因还特别奇怪。在网上找了一大堆教程互相抄来抄去，没一个能用的。最后还是在官网论坛找到了一个答案成功解决问题。

<!--more-->

## 1. 问题描述

我用的是 MATLAB R2016a，当我运行带有 parfor 的代码时，左下角会有如下提示，表示无法启动并行池，而正常情况应该是右边这幅图

| 报错情况                                                     | 正常情况                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![error info](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/matlab-parpool-error.png) | ![normal info](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/matlab-parpool-normal.png) |

当我点击查看 more details 时，会报如下的错误，参数不对？这不是自带函数吗？

![error info](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/matlab-parpool-error2.png)

然后我参考了网上的教程，查看 `Home->Parallel->Manage Cluster Profiles`，但是网上教程说如果是下面这个样子

| 我的错误是这样的                                             | 网上只有这样的                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![my error](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/matlab-parpool-error3.png) | ![others](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/matlab-parpool-error4.png) |

很显然我的第一步就 fail 了，我按照网上的说法运行下面这句话也没用

```matlab
distcomp.feature( 'LocalUseMpiexec', false )
```

最后幸运的是在官网论坛找到了一篇可以解决我的问题的回答，下面给出解决方法。

## 2. 解决方法

> 参考链接：[https://www.mathworks.com/matlabcentral/answers/92124-why-am-i-unable-to-use-parpool-with-the-local-scheduler-or-validate-my-local-configuration-of-parall](https://www.mathworks.com/matlabcentral/answers/92124-why-am-i-unable-to-use-parpool-with-the-local-scheduler-or-validate-my-local-configuration-of-parall)
>
> 我用**第 5 步(Clear the local scheduler data folder)**解决了我的问题，不过下面还是贴出来完整的 debug 过程

### 2.1 检查 license

运行命令检查 `Parallel Computing Toolbox` 的 license 正确

```matlab
license checkout Distrib_Computing_Toolbox
```

如果得到的回答是 `ans=1`，则说明 license 没问题。否则需要添加 license。

### 2.2 确保你的 MATLAB 版本与 PCT 版本匹配

运行命令 `ver` 查看，如果不匹配则无法使用。这种情况 ...... 建议重装。

### 2.3 Disable local mpiexec

运行下面的命令

```matlab
distcomp.feature( 'LocalUseMpiexec', false )
```

这也是网上绝大部分教程的方法，不过对我就不适用，如果对你也不适用的话，往下继续看。

### 2.4 Check your local scheduler configuration

这一部分我贴出原文吧，大概是说如果你修改了默认配置，则可以重置他们。

> There are no changes that need to be made in order to use the local scheduler, but if you have made changes to the configuration, you may want to reset these. This can be done by creating a new local scheduler configuration. To do so,
>
> 1. Go to the Parallel menu in MATLAB and select "Manage Cluster Profiles..." ("Manage Configurations..." for R2011b or earlier) 
> 2. Click on Add > Custom > Local (for older releases: From the File menu, select New > Local Configuration)
> 3. Click the radio option in the default column to set this as the default configuration
>
> Once complete, close the Manage Configuration windows and try again.

### 2.5 Clear the local scheduler data folder

 出现无法启动并行池的原因也可能是本地的 local scheduler data 有问题，可以把他删除。删除的步骤为

1. 运行命令

   ```
   >>prefdir
   ans =
   C:\Users\Administrator\AppData\Roaming\MathWorks\MATLAB\R2016a
   ```

   然后我们就可以在路径 `C:\Users\Administrator\AppData\Roaming\MathWorks\MATLAB` 下面找到文件夹 `local_scheduler_data` 或者 `local_cluster_jobs`

2. 关闭 MATLAB

3. 将上面的文件夹 `local_scheduler_data` 或者 `local_cluster_jobs` 重命名或者直接删除

4. 重启 MATLAB，试着开启并行池

我做完这一步就能解决问题了，如果还不行，原文还给出了其他可能的原因，后面的我就直接贴出来原文了。

### 2.6 Ensure that hostname resolution works on your computer

In order to use the local scheduler, your computer's own hostname must be resolvable. To confirm this, run the following command in MATLAB:

```matlab
!hostname
```

This will give you your computer's hostname. You must be able to resolve this hostname to the computer's IP address. To test this you can run:

```matlab
!ping <hostname>
```

Where <hostname> is the output of the hostname command above. If the results indicate the wrong IP address or say that your computer is an "unknown host", there is a network issue on your computer that needs to be resolved in order to use the local scheduler. In that case, ask your network administrator for help.

### 2.7 Check to see if you have a startup.m file on the MATLAB path

It may be causing an error, even if it works fine in MATLAB when run as code.

To see if you have a startup.m file on the MATLAB path run the below command in MATLAB:

```matlab
which startup.m
```

Either delete or move that file outside of the MATLAB path.

If you are still unable to run parpool, run a validation of your local scheduler configuration and submit this to support. To validate:

1. Go to the Parallel menu in MATLAB and select "Manage Cluster Profiles..." ("Manage Configurations..." for R2011b or earlier)
2. Highlight your local scheduler configuration and click the "Validate" button ("Start Validation" for R201b or earlier)
3. Once the validation completes, click the "details" link to see the results

You can then forward your output of validation, the results of the tests below, and your license number to support here: [http://www.mathworks.com/support/contact_us/index.html](http://www.mathworks.com/support/contact_us/index.html)