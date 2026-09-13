---
title: 树莓派设置开机自启动程序
date: 2020-07-20 17:04:52
tags:
  - raspberry pi
categories:
  - Hardware
---

最近调试树莓派，希望**开机运行两个程序**，其中一个是**可执行文件**，另一个是 **python 脚本**，他们都是无限循环的程序，也就是说不关机不会停止运行。中间还是遇到了很多 bug，现在记录一下自启动程序的设置方法以及debug的整个过程。

## 1. 自启动程序设置方法

网上用的最多的方法就是修改 `/etc/rc.local` 文件：

```shell
sudo nano /etc/rc.local
```

进入之后在 `exit 0` 这句话上面添加需要运行的程序。比如我想运行 `~/test/` 文件夹下的可执行文件 `runme` 和 python 脚本 `runhe.py`，那么就需要添加下面两个命令

```shell
# sleep 5
/home/pi/test/runme &
sudo -H -u pi /usr/bin/python3 /home/pi/test/runhe.py &
```

<!--more-->

这里有几个**注意事项**：

1. 最好都使用**绝对路径**！
2. 如果程序是无限循环（不会终止）的，那么需要在行尾添加 `&`，如果不是的话可以不加这个 `&`，这个符号可以理解为允许当前行的程序在后台运行，这样就可以继续启动下一行的程序了。
3. 第一行的 `sleep 5` 表示先暂停 5s，主要是为了防止有的变量或者环境还没有准备好，可以根据情况决定是否添加。
4. 运行 python 脚本的时候最好前面用 `/usr/bin/python3 xxx.py` 而不是直接 `python3 xxx.py`，后者不一定会报错，但是像前面说的，还是尽量用绝对路径。
5. 最重要的一点，也是我的 bug 原因所在：运行 python 脚本的时候，网上大多数教程说的是添加 `python3 xxx.py` 就行了，但是我 debug 过程中发现必须要使用 `sudo -H -u pi /xxx/python3 xxxx.py` 来显式的指定用户，否则可能会报错 `ModuleNotFoundError: No module named 'XXX' `，这应该是因为某些包只在某个用户环境中安装了。

## 2. debug技巧

按照上面的方法修改 `rc.local` 文件还是有可能失败，这里再记录几个 debug 的方法。

首先是可以利用下面的命令查看是否运行了含有 `runme` 的程序

```shell
ps aux|grep runme
```

另外可以将 `rc.local` 的第一行 `#!/bin/sh` 修改为

```bash
#!/bin/sh -e(或者 -x)
```

这样可以把日志记录到 `/var/log/messages` 文件中，后续可以查看这个文件看看是哪里报错。

然后是每次修改 `rc.local` 之后都要重启来检验有没有问题，太麻烦了，其实还有更高效的方法，只需要在命令行运行

```shell
systemctl restart rc-local
systemctl status rc-local
```

前者模仿开机过程，重新执行一遍 `rc.local` 中的命令，后者查看运行状态。

此外，还可以使用 `nohup` 命令使程序后台运行

```shell
nohup ./calcDynamic &
```

