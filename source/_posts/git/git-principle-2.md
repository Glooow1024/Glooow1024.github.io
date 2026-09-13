---
title: Git 工作原理(二)
date: 2019-12-29 21:51:40
tags: Git
categories: Git
---

> 在上一篇文章[Git工作原理（一）](https://glooow1024.github.io/2019/12/29/git/git-principle-1/#more)中，我们介绍了简单的 git 版本控制，上一篇文章中并没有牵涉到分支 branch 相关的内容，这篇文章将会介绍：当我们创建不同的分支时，git 做了些什么？

<!--more-->

[TOC]

## 1. `master` 分支

上一篇文章结束后，我们查看仓库的历史

```shell
$ git ls-files --stage
100644 445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 0       a.txt
100644 c200906efd24ec5e783bee7f23b5d7c941b0c12c 0       dir/b.txt

$ git cat-file --batch-check --batch-all-objects
0ed6427de6990a17351bf0e0fd648b642e15f967 tree 63
38f74e0a07955212bdb02699f6d73cd7420cd823 commit 175
445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 blob 7
58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4
aef06e3d27cc6b17730daf473499ab58b68e772d tree 33
b00f88d6e09fd9e767fc3246c971bf0d14f0621e commit 223
b02b1164ed5a571b723cb25d978780b15d826d62 tree 63
c200906efd24ec5e783bee7f23b5d7c941b0c12c blob 4

$ git log
commit b00f88d6e09fd9e767fc3246c971bf0d14f0621e (HEAD -> master)
Author: Glooow1024 <glooow1024@gmail.com>
Date:   Sun Dec 29 16:07:08 2019 +0800

    commit 2

commit 38f74e0a07955212bdb02699f6d73cd7420cd823
Author: Glooow1024 <glooow1024@gmail.com>
Date:   Sun Dec 29 15:42:55 2019 +0800

    commit 1
    
$ git cat-file -p b00f
tree 0ed6427de6990a17351bf0e0fd648b642e15f967
parent 38f74e0a07955212bdb02699f6d73cd7420cd823
author Glooow1024 <glooow1024@gmail.com> 1577606828 +0800
committer Glooow1024 <glooow1024@gmail.com> 1577606828 +0800

commit 2
```

当前 `.git/` 文件夹下我们主要关注以下内容

```shell
.git
├── HEAD
├── refs
│   └── tags
│   └── heads
│   	└── master
├── logs
│   └── HEAD
│   └── refs
│   	└── heads
│   		└── master
...
```

### 1.1 `.git/HEAD`

如果查看当前的 `HEAD` 就可以看到他指向了仓库 `master` 最后一次 `commit`

```shell
$ cat .git/HEAD
ref: refs/heads/master

$ cat .git/refs/heads/master
b00f88d6e09fd9e767fc3246c971bf0d14f0621e
```

### 1.2 `.git/logs`

如果再查看 `.git/logs/` 中的文件，可以看到这里也保存一个 `HEAD`，但是内容跟之前差很多

```shell
$ cat ./.git/logs/HEAD
0000000000000000000000000000000000000000 38f74e0a07955212bdb02699f6d73cd7420cd823 Glooow1024 <glooow1024@gmail.com> 1577605375 +0800    commit (initial): commit 1
38f74e0a07955212bdb02699f6d73cd7420cd823 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577606828 +0800    commit: commit 2

```

第一列是上一次提交的 `commit` 哈希值，第二列是本次 `commit` 哈希值，后面是用户信息，最后一列则是每次 `commit` 的附加的消息 message。

再看 `logs` 中的 `master`，可以看到跟前面 `HEAD` 中的内容一样 

```shell
$ cat .git/logs/refs/heads/master
0000000000000000000000000000000000000000 38f74e0a07955212bdb02699f6d73cd7420cd823 Glooow1024 <glooow1024@gmail.com> 1577605375 +0800    commit (initial): commit 1
38f74e0a07955212bdb02699f6d73cd7420cd823 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577606828 +0800    commit: commit 2
```

## 2. Git 的分支管理

### 2.1 `git branch`

现在让我们新建一个分支

```shell
$ git branch br1
$ cat ./.git/HEAD
ref: refs/heads/master
```

`HEAD` 的内容当然没有改变，因为我们没有转移到新的分支。但是 `.git/` 发生了哪些变化呢？他变成了这样

```shell
.git
├── HEAD
├── refs
│   └── tags
│   └── heads
│   	└── br1
│   	└── master
├── logs
│   └── HEAD
│   └── refs
│   	└── heads
│   		└── br1
│   		└── master
...
```

来看看内容

```shell
$ cat ./.git/refs/heads/br1
b00f88d6e09fd9e767fc3246c971bf0d14f0621e

$ cat .git/logs/refs/heads/br1
0000000000000000000000000000000000000000 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577615247 +0800    branch: Created from master
```

`refs/` 中的 `br1` 只是记录了产生分支的最近一次 `commit` 的哈希值，而 `logs/` 中的信息第一列变成了空，也就是说分支后不能向更早的版本回退，最多回退到分支时的那个版本，同时最后一项信息记录了这个分支信息。

> **敲黑板！重点**
>
> - `.git/refs/master` 只记录了当前分支最后一个 `commit` 的哈希值；
> - `.git/logs/refs/master` 记录了当前分支所有 `commit` 的哈希值，构成一个可以向旧版本回溯的单向链表；
> - `.git/HEAD` 只记录了当前工作区所在分支对应的 `refs/` 中的文件；
> - `.git/logs/HEAD` 记录了 `.git/HEAD` 的变化历程；

### 2.2 `git checkout`

现在让我们转移到新的分支

```shell
$ git checkout br1
Switched to branch 'br1'

$ cat ./.git/HEAD
ref: refs/heads/br1

```

只是修改了当前的 `HEAD` 就表示我们转移到了新的分支，原来的主分支 `master` 还在 `ref: refs/heads/master` 中保存，所以不会有信息丢失。

假如我们在分支上修改了内容呢？

```shell
$ echo 'hello' >> a.txt
$ cat a.txt
hahaha
hello

```

然后查看暂存区 `index` 文件，可以看到 `a.txt` 的哈希值已经改变了

```shell
$ git ls-files --stage
100644 63db897b879aac027311451ea6d8158daab3ac39 0       a.txt
100644 c200906efd24ec5e783bee7f23b5d7c941b0c12c 0       dir/b.txt

```

### 2.3 `git commit`

然后我们提交一哈，不出意外的话用 `$ git cat-file --batch-check --batch-all-objects` 会发现多了一个 `commit` 和一个 `tree` 文件，这是我们在上一篇文章中所讲的，忘记的可以再回顾一下。

再看当前 `br1` 已经被修改了

```shell
$ cat .git/refs/heads/br1
4b60ac6ea7ebab972920f84bd07de3d20d7d5804

$ cat .git/logs/refs/heads/br1
0000000000000000000000000000000000000000 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577615247 +0800	branch: Created from master
b00f88d6e09fd9e767fc3246c971bf0d14f0621e 4b60ac6ea7ebab972920f84bd07de3d20d7d5804 Glooow1024 <glooow1024@gmail.com> 1577616495 +0800	commit: commit br 1

```

重要的是 `logs/HEAD`

```shell
$ cat .git/logs/HEAD
0000000000000000000000000000000000000000 38f74e0a07955212bdb02699f6d73cd7420cd823 Glooow1024 <glooow1024@gmail.com> 1577605375 +0800	commit (initial): commit 1
38f74e0a07955212bdb02699f6d73cd7420cd823 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577606828 +0800	commit: commit 2
b00f88d6e09fd9e767fc3246c971bf0d14f0621e b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577615776 +0800	checkout: moving from master to br1
b00f88d6e09fd9e767fc3246c971bf0d14f0621e 4b60ac6ea7ebab972920f84bd07de3d20d7d5804 Glooow1024 <glooow1024@gmail.com> 1577616495 +0800	commit: commit br 1

```

可以看到这里记录了我们转移分支并提交的记录。

### 小结

> **敲黑板！重点**
>
> 1. 实质上我们的每个 branch 都相当于维护了一个 `commit` 文件的单项链表；
> 2. 命令 `git branch` 实际上就是在 `.git/refs/heads` 和 `.git/logs/refs/heads` 分别创建一个对应的文件，文件名就是分支名，文件中保存了这个分支对应的 `commit` 链表的各项；
> 3. 我们通过 `git checkout` 实际上就是修改 `.git/HEAD` 使其指向对应的分支在 `.git/refs/` 中的文件；

### 2.4 `git merge`

现在让我们合并一下分支

```shell
$ git merge br1
Updating b00f88d..4b60ac6
Fast-forward
 a.txt | 1 +
 1 file changed, 1 insertion(+)
```

我们再来看一下 `HEAD` 文件

```shell
$ cat .git/logs/HEAD
0000000000000000000000000000000000000000 38f74e0a07955212bdb02699f6d73cd7420cd823 Glooow1024 <glooow1024@gmail.com> 1577605375 +0800    commit (initial): commit 1
38f74e0a07955212bdb02699f6d73cd7420cd823 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577606828 +0800    commit: commit 2
b00f88d6e09fd9e767fc3246c971bf0d14f0621e b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577615776 +0800    checkout: moving from master to br1
b00f88d6e09fd9e767fc3246c971bf0d14f0621e 4b60ac6ea7ebab972920f84bd07de3d20d7d5804 Glooow1024 <glooow1024@gmail.com> 1577616495 +0800    commit: commit br 1
4b60ac6ea7ebab972920f84bd07de3d20d7d5804 b00f88d6e09fd9e767fc3246c971bf0d14f0621e Glooow1024 <glooow1024@gmail.com> 1577625498 +0800    checkout: moving from br1 to master
b00f88d6e09fd9e767fc3246c971bf0d14f0621e 4b60ac6ea7ebab972920f84bd07de3d20d7d5804 Glooow1024 <glooow1024@gmail.com> 1577625944 +0800    merge br1: Fast-forward
```

这里的 `merge` 过程实际上就是把 master 对应的指针移到了 `br1` 对应的指针处，看下图很容易理解（图片来自于 https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6）

![git branch](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/basic-branching-3.png)

删除分支实质上也就是删除了分支文件 `.git/refs/heads/br1` 和 `.git/logs/refs/heads/br1` 

```shell
$ git branch -d br1
```

## 3. Git 的标签管理

我们先看一下

```shell
$ git log --pretty=oneline
4b60ac6ea7ebab972920f84bd07de3d20d7d5804 (HEAD -> master) commit br 1
b00f88d6e09fd9e767fc3246c971bf0d14f0621e commit 2
38f74e0a07955212bdb02699f6d73cd7420cd823 commit 1
```

### 3.1 `git tag`

打个标签

```shell
$ git tag -a v0.1 b00f
```

然后就会发现 `.git/refs/tags/` 中多了一个 `v0.1` 文件，实际上就是一个指针（`commit` 文件的哈希值），跟 `.git/refs/heads/master` 中的指针没有区别

```shell
$ cat .git/refs/tags/v0.1
3ad673290fa12aecc5bb66e7c7d3f83157914957
```

需要注意的是增加 tag 并不会修改 `.git/logs` 中的文件，因为这个文件夹是维护版本更新历史的，打标签并没有产生新的版本。