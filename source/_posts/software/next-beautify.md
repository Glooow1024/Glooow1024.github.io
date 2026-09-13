---
title: Hexo NexT 7.7 主题美化
date: 2020-03-11 16:07:05
tags:
  - hexo
  - next
categories:
  - Hexo
---

网上很多关于主题美化的教程都是老版本 next 5.1 的，最近更新到 next 7 之后摸索了好久才找到简单有效的自定义主题方式，下面是具体的操作。

<!--more-->

修改主题下 `_config.yml` 文件，找到下面这一部分，也即注释掉最后一行

```
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  #footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  style: source/_data/styles.styl
```

然后在**博客根目录**下创建文件 `blog/source/_data/styles.styl`，注意是博客根目录而不是主题下的目录，然后我们就可以在这个文件里边添加自定义配置。

> 注意想自定义博客外观的话尽量都在这个文件里修改，不要修改其他原始文件，毕竟这个修改坏了删掉就是了，后面所有的修改都是在这一个文件里边添加内容，是不是很简答呢？

```
// 修改背景图片
body {
    background: url(/images/background/blue.jpg);
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-position: 50% 50%;
}

// 修改主体透明度
// 这个设置并不好使，会使整个页面蒙上一层不透明白色图层，不建议使用
// 建议使用下面的 .post-block 与 .comments
//.main-inner {
//    background: #fff;
//    opacity: 0.8;
//}

// 修改文章块透明度
.post-block {
	padding: $content-desktop-padding;
	background: rgba(255,255,255,0.9);   //white;
	box-shadow: $box-shadow-inner;
	border-radius: $border-radius-inner;
}

// Comments blocks.
.comments {
	padding: $content-desktop-padding;
	margin: initial;
	margin-top: sboffset;
	background: rgba(255,255,255,0.9);   //white;
	box-shadow: $box-shadow;
	border-radius: $border-radius;
}

// 修改菜单栏透明度
.header-inner {
    opacity: 0.8;
}

// 设置主页面宽度
.header{
    width: 90%;
    +tablet() {
        width: 100%;
    }
    +mobile() {
        width: 100%;
    }
}
.container .main-inner {
    width: 90%;
    +tablet() {
        width: 100%;
    }
    +mobile() {
        width: 100%;
    }
}
.content-wrap {
    width: calc(100% - 260px);
    +tablet() {
        width: 100%;
    }
    +mobile() {
        width: 100%;
    }
}
```

