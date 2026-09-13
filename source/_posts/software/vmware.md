---
title: VMWare15 + Ubuntu18.04 踩坑记录
date: 2020-08-11 23:02:20
tags:
  - 虚拟机
  - linux
categories:
  - Software
---

## 1. 虚拟机联网问题

VMWare虚拟机大致有 3 种联网方式，以下三种方式联网自由度逐渐递减：

1. 桥接：虚拟机就相当于局域网中的另一台主机，有独立的 ip 地址；
2. NAT：虚拟机需要借助于主机才能联网，虚拟机也是以物理主机的身份与外界通信；
3. Host-only：虚拟机只能与主机通信，不能联网；

有时候设置好 NAT 或者桥接模式后仍然不能联网，可以尝试输入以下命令

```shell
sudo service network-manager restart
```

也可以尝试先把虚拟机关机，点击 VMWare `编辑 >> 更改设置 >>还原默认设置 >> 确定 `，然后虚拟机开机就可以了。

找不到网络连接图标

```shell
sudo service network-manager stop
sudo rm /var/lib/NetworkManager/NetworkManager.state
sudo service network-manager start
# 将文件里面唯一的false改成true
sudo gedit /etc/NetworkManager/NetworkManager.conf
sudo service network-manager restart
```

## 2. vi 输入问题

vi 输入模式下方向键会出来 A, B, C, D，而且退格键不好使。

### 方法 1

可以修改文件 `/etc/vim/vimrc.tiny`，注释掉原来的 `set compatible`，改成以下内容

```bash
set nocompatible
set backspace=2
```

### 方法 2

在用户个人目录下创建文件 `.vimrc`，写入以下内容

```bash
set nocompatible     # 以非兼容模式工作  
set backspace=2
```

### 方法 3

安装 vim

```shell
sudo apt-get install vim
```

