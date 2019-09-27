---
title: github使用方法
date: 2019-05-25 12:22:37
update: 2019-05-25 12:22:37
tag:
- github
- git bash
categories:
- note
---

# GitHub的使用方法

发现自己很容易忘记Github怎么用，就写一个备忘录吧。

在windows平台上使用git bash。

已经配置好了ssh key。

<!-- more -->

## 基本信息

* 所有的repo都放在了`E:/documents/Github`里面了。
* 已经配置好了SSH
* 用户名为`sidpku`

## 基本使用方法

### 创建库

我一般都是在Github直接创建库，然后pull到本地。

* 首先在Github上创建好库，让后获取url (using SSH&phrase)

* 在本地使用Git bash

```bash
$ dir_github #alias命令，进入存放repo的目录
$ git clone [url]
```

完成。

### 添加上传文件

* 将文件复制到repo中

* 在repo目录中使用git bash

```bash
$ git pull #首先获取最新版本，防止发生冲突
$ git add . #添加文件
$ git commit -m "add existing files"
$ git push origin master #将其加入到master中去。
```

就是这么简单。

## 如何配置git和GitHub

在wikimedia的建站日志中有比较详细的介绍，以后再搬运过来吧。