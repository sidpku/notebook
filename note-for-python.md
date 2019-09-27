---
title: python学习笔记
date: 2019-05-25 20:55:23
update: 2019-05-25 20:55:23
tag:
- python
- programming language
categories:
- note
---

# python学习日记

## 基础语句

python不使用分号`;`作为句尾，而是使用缩进控制。

<!-- more -->

### 文件读写

#### 读文件

```python
with open(FILENAME) as f:
	content=f.readlines()
```

> FIILENAME

* 文件名变量
* [EX] : `'raw.txt'`
  * 需要带引号

> `readlines()`

* 返回列表，每行为一列。

#### 写文件

```python
with open(FILENAME,mode="w") as f:
    f.write(content)
```

* mode
  * w：写文件，如果不存在则创建，若存在则重置。
* FILENAME
  * 引号括起来，windows记得加后缀。

### 循环

#### for循环

```python
for i in line:
    i......
```

#### Continue

跳过本次循环剩余的部分，仍在循环体中。

#### Break

跳出本次循环，不执行本次循环剩余部分，跳出循环体。

## 小Trick

### 循环标签

```python
switch = 0
for i in line:
    if i == "@" :
        switch = 1 - switch 
```



