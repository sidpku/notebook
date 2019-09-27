---
title: note for html&css
date: 2019-03-09 22:16:11
update: 2019-03-09 22:16:11
tag:
- html
- css
categories:
- note
---

# CSS&HTML学习笔记

初衷只是想修改下typora的字体....结果发现想要自定义难度还挺大的，一顿捯饬之后没有进展，网上的教程也很少，就准备自己摸索。先从CSS&HTML语言的基础学起，这个笔记就是用来记录学习到的知识的。

期待自己能早日学会，然后就可以自己把typora修改的漂漂亮亮啦~

<!-- more -->

## HTML

第一部分:HTML的学习，这一部分是CSS的基础。

学习工具：使用 W3CSCHOOL进行学习

### 基本概念

**HTML**: hyper text markup language	超文本标记语言

**markup tag**: 标记标签，为一对，分别为开始标签和结束标签，也被称为开放标签和闭合标签。要求标签使用小写。（规范）

```html
<body></body>
```

**元素**: HTML元素是指一对标签及其内部的所有代码。

> HTML使用一系列markup tag描述网页，**web浏览器**可以读取HTML文件，并按照markup tag将文本内容展现出来。

#### 空元素

我们可以使用`<br/>`这样的标签，不需要成对。

#### 属性

属性可以对文本内容进行进一步的修饰。

* 属性写在开始标签中。

* 属性通过键值对`key="value"`的方式进行定义。
  * 必须将值用引号标记，通常使用双引号`"`
  * 含双引号的值需要用单引号标记。[EX]`text='my name is "alice".'`

* 属性对大小写不明感，但是规范使用小写.

```html
<a href="http://sidpku.github.io"> sidpku`s的网站 </a>
```

[html中可以使用的全局属性_w3cschool](http://www.w3school.com.cn/tags/html_ref_standardattributes.asp)

### 语法

#### 基础语法

* `<html>` 与`</html>`之间表示文本描述页面
* `<body>`与`</body>`表示文本是可见的页面内容

```html
<html>
    <body>
        <h1>
            这是一级标题
        </h1>
        <p>这里是第一段</p>
        <h2>
            这是二级标题
        </h2>
        <p>
            这是第二段
        </p>
    </body>
</html>
```

同时我们要注意到

```html
<p>这是第一段</p>
<p> 这是第一段</p>
```

上面这两句在编译结果上并没有任何区别。

#### 链接

HTML通过`<a>`来指定链接地址

```html
<a href="http://sidpku.github.io"> sidpku`s的网站 </a>
```

#### 图片

HTML通过属性的方式提供属性的名称和尺寸

```html
<img src="hello.jpg" widthn="200" height="100"/>
```

#### 标题

使用`<h1>`&`<\h1>`来表示标题

* 标题一共有6级
* 标题用于生成文章结构，谨慎使用，不要用在加粗上
* 标题前后会自动添加空行

分割线

使用`<hr/>`添加分割线

#### 添加注释

通过`<!-- comment -->`添加注释

#### 水平线

```
<hr/>
```



#### 空行/换行

```
<p1>这是段落的第一行<br/>这是段落的第二行</p1>
```



#### 属性设置

##### 居中

```html
<h1 style="text-align:center"></h1>
```

##### 背景颜色

```html
<body style="background-color:yellow"></body>
```

##### 字体、大小、颜色

注意`;` & `:`的使用。

```html
<p style="font-family:arial;color:red;font-size:20px;">A paragraph.</p>
```

###### 颜色

字体颜色

**使用颜色名**

```html
<p style="color:red">The color is Red</p>
```

**使用十六进制**

```php+HTML
<p style="color:#F0F8FF">The color is AliceBlue</p>
```

可以参考的网站

[1.颜色选择辅助网站](<https://htmlcolorcodes.com/zh/>)



#### 文本格式化

