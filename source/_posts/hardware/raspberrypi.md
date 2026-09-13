---
title: 树莓派使用
date: 2020-06-08 17:33:20
tags:
  - raspberry pi
categories:
  - Hardware
---

## 1. 系统安装

[参考链接](https://zhuanlan.zhihu.com/p/59027897)

## 2. 连接树莓派

如果有显示器和键盘鼠标，可以直接利用HMDI线连接树莓派和显示器，像操作普通电脑一样。

如果没有显示器，就需要通过ssh连接。首先需要知道树莓派的地址。

<!--more-->

### 2.1 方法一：wifi

1. 在树莓派的 SD 卡里面 `boot` 分区下面创建文件 `wpa_supplicant.conf`，写入以下内容

   ```
   country=CN
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
    
   network={
       ssid="wifi名"
       psk="密码"
       key_mgmt=WPA-PSK
   }
   ```

2. 同样在 `boot` 分区里面创建一个空文件，重命名为 `ssh`，注意不是 `ssh.txt`，没有后缀！！！

3. 树莓派上电，可以在浏览器输入 `192.168.0.1` 查看树莓派地址，一般为 `192.168.0.x`

4. 用 putty 或者其他软件 ssh 远程连接，默认用户名 `pi`，密码 `raspberry`

5. 可以在命令行输入 `sudo raspi-config` 打开 VNC、修改分辨率

### 2.2 方法二：网线

可以用网线直接把树莓派和笔记本电脑连接在一块。之后需要打开 `控制面板 -> 网络和 Internet -> 网络和共享中心`，不出意外的话可以看到下面的东西

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/net.PNG)

点击 `WLAN -> 属性 -> 共享`，  如下图勾选对号并且点击 `设置` 勾选 `1703` ，点击 `确定`。

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/net2.PNG)

再点击 `以太网 -> 详细信息`，看到那个 `IPv4地址`，记住他 `192.168.137.1`。

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/net4.PNG)

然后打开命令行 `win+R -> cmd`，输入 `arp -a`，就可以看到下面的东西，找到那个接口 `192.168.137.1`（就是上面记住的那个地址），看下面的 IP 地址（我这里是因为连接了两次，所以分配了两次地址 `192.168.137.43 or 79`，如果是第一次连接的话应该只有一个地址）就是树莓派的地址。

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/net5.PNG)

获得这个 IP 地址以后就能跟方法一中一样，用 ssh 连接树莓派了。

## 3. 修改系统镜像源

1. 备份原文件

   ```
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak 	
   sudo cp /etc/apt/sources.list.d/raspi.list /etc/apt/sources.list.d/raspi.list.bak
   ```

2. 修改软件更新源

   ```
   sudo vi /etc/apt/sources.list
   ```

   输入以下内容

   ```
   deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
   deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
   ```

3. 修改系统更新源

   ```
   sudo vi /etc/apt/sources.list.d/raspi.list
   ```

   输入以下内容

   ```
   deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
   ```

4. 同步更新源及更新软件包

   ```
   sudo apt-get update
   sudo apt-get upgrade
   ```

## 4. 修改 pip 镜像源

**临时更换源**只需要在命令后面加一个参数：

```shell
pip install django -i http://pypi.douban.com/simple
```

**永久更换源**可以创建文件 `~/.pip/pip.conf` 并写入以下内容：

```shell
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
```

上面是选择的清华源，还有以下国内镜像源可以选择：

- 清华：https://pypi.tuna.tsinghua.edu.cn/simple
- 阿里云：http://mirrors.aliyun.com/pypi/simple/
- 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
- 华中理工大学：http://pypi.hustunique.com/
- 山东理工大学：http://pypi.sdutlinux.org/
- 豆瓣：http://pypi.douban.com/simple/

## 5. 串口通信

树莓派需要手动打开串口，可以在命令行输入 `sudo raspi-config` 选择 `interface options` 打开 `Serial`。树莓派 4B 的针脚如图所示，对于 USB 转串口模块只需要接树莓派的 6(GND),8(TXD),10(RXD) 口即可，注意 USB 转串口的 RXD 要与树莓派的 TXD 连接。

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/pin-raspi.png)

