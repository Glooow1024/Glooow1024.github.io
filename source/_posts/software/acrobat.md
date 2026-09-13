---
title: Adobe Acrobat Pro DC更新后提示登录激活问题
date: 2002-08-05 20:17:03
tags:
  - Adobe
categories:
  - Software
---

Adobe Acrobat Pro DC 更新之后不能直接用 AMTEmu v0.9.2 激活了。

不过只需要修改以下注册表再重新激活就可以了。

通过 `win+R` 输入 `regedit` 打开注册表，在以下位置处

```
[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Adobe\Adobe Acrobat\DC\Activation] 
```

创建一个 `DWORD(32位)` 类型的项，数值为十六进制 0x00000001。

然后就可以用 AMTEmu v0.9.2 重新激活了。

