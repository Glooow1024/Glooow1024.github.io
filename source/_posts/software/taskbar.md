---
title: Windows任务栏右侧小图标显示不完整
date: 2020-01-08 17:58:44
tags: 
  - windows
categories:
  - Software
---

<!--more-->

之前发现windows电脑右侧小图标显示不完整

![before](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/taskbar_before.png)

百度了一下，只需要新建一个`bat`文件，内容是

```bash
cd /d %userprofile%\AppData\Local\Microsoft\Windows\Explorer
taskkill /f /im explorer.exe
attrib -h iconcache_*.db
del iconcache_*.db /a
start explorer
pause
```

以管理员身份运行后就好了，中间屏幕会闪一下蓝屏2s，不用担心。然后就可以变成下面这样

![after](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/taskbar_after.png)

参考资料：https://jingyan.baidu.com/article/ae97a64608ba0cbbfd461d8f.html