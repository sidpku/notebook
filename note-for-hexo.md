---
title: HEXO使用方法
date: 2019-05-25 11:30:02
update: 2019-05-25 11:30:02
tag:
- hexo
- next
categories:
- note
---

# HEXO 的使用方法

发现自己的记性实在是不好，2个月没有用hexo写点东西，就忘记了如何用hexo和git了，所以我还是把自己的常用操作都记录下来吧。

在windows平台上使用git bash进行hexo博客的发布。

<!--more-->

## 如何发布新博客

发布博客前，首先要进入到hexo的工作目录。

```bash
$ dir_blog
```

这是因为我在git bash中设置了alias变量:

```bash
$ alias dir_blog="cd /e/document/blogs/Hexo
```

### 创建新的blog

```bash
$ hexo new [type] <title>
[EX]：
$ hexo new post note-for-hexo
```

### 发布前首先在本地测试

```bash
$ hexo s
```

或者使用我自己编好的bash文件

```bash
$ bash test.sh
```

### 发布文章

```bash
$ bash update.sh
```

发布文章也是用自己编好的bash文件来做。

### 如何插入图片？

首先默认已经配置好了hexo & next，所以此处只写插入图片的方法，不写配置。

1. 将图片复制到`_post`文件夹下，对应该post的图片文件夹
2. 在post中写好相对路径

[EX]    `/note-for-matlab/mesh和surf函数的区别.jpg`

### 如何写数学公式？

* 在post中，按照markdown & latex 规范书写行间、行内公式。
* 在post中，将抬头的`mathjax： flase`修改为`true`。
* 渲染工具为`MathJax`，数学公式规范可参考该工具的支持，基本与latex一致。

具体配置见：[建站日志](https://sidpku.github.io/2019/03/05/建站日志/)

## 碰到了问题

### 为什么我的post看不到？

可能是因为categories和tag写的不规范。比如categories写成了这样。

```markdown
categories:
-note
```

其实应该写成这样

```markdown
categories:
- note
```

