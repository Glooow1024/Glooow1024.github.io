---
title: Git 工作原理(一)
date: 2019-12-29 16:38:00
tags: Git
categories: Git
---

今天讲一下 Git 工作原理，本文主要目的不是讲解如何使用各种 git 命令，而是关于一些常用 git 命令之后仓库中会出现什么变化，以及 git 是如何进行仓库的版本控制的。

<!--more-->

[TOC]

## 1. Git 的文件管理

### 1.1 Git 文件存储

git 是怎么管理文件的呢？首先我们需要理解 git 对文件的保存逻辑，这很重要：

> **git 根据文件内容来管理文件，而不是文件名**！
>
> 比如某个仓库中，完全相同的两个文件我们保存了 2 份，只不过文件名不同，那么在 git 看来这两个是同一个文件，他会根据文件内容经过SHA1算法计算出一个对应的哈希值，然后根据哈希值来索引文件。
>
> 注：这么做的好处是显而易见的，相同的两个文件不再需要多存储一份。

这是怎么体现出来的呢？其实 git 会把我们仓库中的原始文件压缩成二进制文件，然后都保存在 `.git/objects/` 文件夹下，每个文件的命名就是计算出来的哈希值。我们可以用如下命令来查看 git 给我们保存的所有文件。

```shell
git cat-file --batch-check -batch-all-objects
```

现在我们新建一个仓库试试吧

```shell
$ git init
$ echo '111' > a.txt
mkdir dir
$ echo '222' > dir/b.txt
```

好，我们来看看 `.git/objects/` 文件夹。咦？怎么是空的？什么也没有。哦我们只是修改了工作区，还没添加修改呢，来添加一下

```shell
$ git add .
```

好，现在来看看 `.git/objects/` 吧

```shell
$ git cat-file --batch-check --batch-all-objects
58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4
c200906efd24ec5e783bee7f23b5d7c941b0c12c blob 4
```

### 1.2 Git 文件类型

应用上述命令后，三列分别表示文件的哈希值、**文件类型**、长度。注意这里出现了 git 的文件类型，主要有 4 种：`blob`、`tree`、`commit`、`tag`。

我们可以用以下命令查看文件类型

```shell
git cat-file -t 58c9
```

可以用以下命令查看内容

```shell
git cat-file -p 58c9
```

前面我们只看到了 `blob` 文件，什么情况下会出现 `tree` 和  `commit` 呢？注意我们现在只是新建 txt 并 add 了，还没有提交，那我们 commit 一下吧

````shell
$ git commit -m "commit 1" 
````

然后再查看一下 `.git/objects/` 文件夹

```shell
$ git cat-file --batch-check --batch-all-objects
38f74e0a07955212bdb02699f6d73cd7420cd823 commit 175
58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4
aef06e3d27cc6b17730daf473499ab58b68e772d tree 33
b02b1164ed5a571b723cb25d978780b15d826d62 tree 63
c200906efd24ec5e783bee7f23b5d7c941b0c12c blob 4
```

咦，他们出现了！！！让我们分别看看他们是什么东西~

#### 1.2.1 `blob` 文件

如果是 `blob` 文件我们可以直接看到文件的内容

```shell
$ git cat-file -p 58c9
111
```

#### 1.2.2 `tree` 文件

如果是 `tree` 文件我们可以看到目录信息

```shell
$ git cat-file -p b02b
100644 blob 58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c    a.txt
040000 tree aef06e3d27cc6b17730daf473499ab58b68e772d    dir
```

四列内容分别为文件权限、文件类型、哈希值、文件名。再查看最后一个 `tree` 文件的内容

```shell
$ git cat-file -p aef0
100644 blob c200906efd24ec5e783bee7f23b5d7c941b0c12c    b.txt
```

这不就是我们工作空间中的文件结构嘛！

> 注意我们工作空间中的文件对应的**文件名**和权限等信息是保存在 `tree` 类型的文件中，而不是 `blob` 文件中哦！

#### 1.2.3 `commit` 文件

让我们再看看这个 `commit`

```shell
$ git cat-file -p 38f7
tree b02b1164ed5a571b723cb25d978780b15d826d62
author Glooow1024 <glooow1024@gmail.com> 1577605375 +0800
committer Glooow1024 <glooow1024@gmail.com> 1577605375 +0800

commit 1
```

其实 `commit` 就是指向了当前仓库的根目录所对应的 `tree` 文件嘛。

#### 1.2.4 `tag` 文件

这个我们以后再说。

#### 1.2.5 `index` 文件

细心的话可以发现我们 add 以后 `.git/` 文件夹下的 `index` 文件也会被修改，我们可以用以下命令查看他的内容

```shell
$ git ls-files --stage
100644 58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c 0       a.txt
100644 c200906efd24ec5e783bee7f23b5d7c941b0c12c 0       dir/b.txt
```

其实它的内容跟 `tree` 是类似的，不过多了一列，这个我们后面再说

#### 小结

> **敲黑板**！各种文件的作用是
>
> - `blob`：工作区的任何文件都会被 git 压缩后以 `blob` 形式保存一个副本在 `.git/objects/` 文件夹下，文件名就是计算的哈希值；
> - `tree`：这个其实就是目录文件，描述当前文件夹结构；注意文件名和权限等信息是保存在 `tree` 文件中而不是 `blob` 文件，后者只保存文件内容；
> - `commit`：记录提交的信息，指向仓库当前根目录的 `tree` 文件；
> - `tag`：记录标签信息。

## 2. Git 的版本控制

### 2.1 `git add` 会发生什么

假如我们现在修改了其中一个文件呢？比如

```shell
$ echo 'hahaha' > a.txt
$ git add .
```

再看一下 `.git/objects/` 文件夹

```shell
$ git cat-file --batch-check --batch-all-objects
38f74e0a07955212bdb02699f6d73cd7420cd823 commit 175
445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 blob 7
58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4
aef06e3d27cc6b17730daf473499ab58b68e772d tree 33
b02b1164ed5a571b723cb25d978780b15d826d62 tree 63
c200906efd24ec5e783bee7f23b5d7c941b0c12c blob 4
```

增加了什么？

```shell
445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 blob 7
```

我们来看看这个文件，他就是一个 `blob` 文件

```shell
$ git cat-file -p 445a
hahaha
```

看来就是把我们修改后的文件又存储了一个 `blob` 文件，注意修改前的文件并没有删除哦，也就是修改前的 `a.txt` 对应的 `58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4` 还在，方便我们以后版本回退嘛。

那么我们再看看 `index` 文件

```shell
$ git ls-files --stage
100644 445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 0       a.txt
100644 c200906efd24ec5e783bee7f23b5d7c941b0c12c 0       dir/b.txt
```

咦，`a.txt` 文件的指针已经修改了！！！指向了最新的 `blob` 文件。好了我们知道了

> `index` 文件在执行 `git add` 之后就会被修改，总是保存最新的仓库根目录信息。事实上，这也**是我们下次 `commit` 所要提交的信息**！！！
>
> 事实上，`index` 就是我们常说的 git 的**暂存区**！！！

### 2.2 `git commit` 会发生什么

如果再提交一下修改呢？

```shell
$ git commit -m "commit 2"
$ git cat-file --batch-check --batch-all-objects
0ed6427de6990a17351bf0e0fd648b642e15f967 tree 63
38f74e0a07955212bdb02699f6d73cd7420cd823 commit 175
445a69c00e48288ac420a2ead9ae5a1cb4cd36d4 blob 7
58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c blob 4
aef06e3d27cc6b17730daf473499ab58b68e772d tree 33
b00f88d6e09fd9e767fc3246c971bf0d14f0621e commit 223
b02b1164ed5a571b723cb25d978780b15d826d62 tree 63
c200906efd24ec5e783bee7f23b5d7c941b0c12c blob 4
```

好了，新增加的有

```shell
0ed6427de6990a17351bf0e0fd648b642e15f967 tree 63
b00f88d6e09fd9e767fc3246c971bf0d14f0621e commit 223
```

我们可以很容易推断，新的 `tree` 文件就描述了更新后的根目录信息，可以看出 `a.txt` 文件的指针（哈希值）变了，但是由于 dir 文件夹中的内容没有任何修改，所以他的指针不变

```shell
$ git cat-file -p 0ed6
100644 blob 445a69c00e48288ac420a2ead9ae5a1cb4cd36d4    a.txt
040000 tree aef06e3d27cc6b17730daf473499ab58b68e772d    dir
```

我们再看看新的 `commit` 文件，可以发现还多了一个 `parent` 选项，也就是指向了上一次提交对应的的 `commit` 文件。

```shell
$ git cat-file -p b00f
tree 0ed6427de6990a17351bf0e0fd648b642e15f967
parent 38f74e0a07955212bdb02699f6d73cd7420cd823
author Glooow1024 <glooow1024@gmail.com> 1577606828 +0800
committer Glooow1024 <glooow1024@gmail.com> 1577606828 +0800

commit 2
```

### 小结

> **重点来了，敲黑板**！
>
> - `git add`
>   - 对每个修改后的文件压缩，并保存一个新的 `blob` 文件在 `.git/objects/` 文件夹下，用哈希值命名文件；
>   - 修改 `.git/index` 保存最新的根目录信息；
> - `git commit`
>   - 生成新的 `tree` 文件保存在 `.git/objects/` 目录下，记录新的仓库文件结构信息；
>   - 生成新的 `commit` 文件保存在 `.git/objects/` 目录下，指向当前最新的根目录的 `tree` 文件；同时该文件中还存在以一项 `parent` 指向上一次的 `commit` 文件；
>
> 实际上，只需要**1 个 `commit`，若干个 `tree` 和若干个 `blob` 文件**就可以**完整描述**仓库的当前提交版本。因此 git 只需要用一个单向链表记录 `commit` 文件之间的指向关系，就可以描述版本变化。

