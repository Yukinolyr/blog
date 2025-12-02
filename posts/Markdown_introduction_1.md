---
title: '简易的Markdown语法教程'
date: '2025-12-01 18:45:59'
categories:
  - 指南
tags:
  - Coding
thumbnail: 'images/markdown_logo.jpeg'
---

## Markdown 简介

<u>Markdown</u> 是一种轻量级的标记语言，可用于在纯文本文档中添加格式化元素，由 John Gruber 于 2004 年创建，如今已成为世界上最受欢迎的标记语言之一。

- 专注于文字内容；
- 纯文本，易读易写，可以方便地纳入版本控制；
- 语法简单，没有什么学习成本，能轻松在码字的同时做出美观大方的排版。

使用 Markdown 与使用 Word 类编辑器不同。在 Word 之类的应用程序中，单击按钮以设置单词和短语的格式，并且，更改立即可见。而 Markdown 与此不同，当你创建 Markdown 格式的文件时，可以在文本中添加 Markdown 语法，以指示哪些单词和短语看起来应该有所不同。例如，要表示标题，只须在短语前面添加一个井号即可（例如， `# Heading One`）。

## Markdown语法教程

### 标题

不同数量的`#`可以完成不同的标题，如下：

``` markdown
# 一级标题

## 二级标题

### 三级标题
```

### 字体

粗体、斜体、粗体和斜体，删除线，需要在文字前后加不同的标记符号。如下：

**这个是粗体**
``` markdown
**这个是粗体**
```

*这个是斜体*
``` markdown
*这个是斜体*
```

***这个是粗体加斜体***
``` markdown
***这个是粗体加斜体***
```

~~这里想用删除线~~
``` markdown
~~这里想用删除线~~
```

> 如果想给字体换颜色、字体或者居中显示，需要使用内嵌HTML来实现。

### 无序列表

无序列表的使用，在符号`-`后加空格使用。如下：

- 无序列表 1
- 无序列表 2
- 无序列表 3
``` markdown
- 无序列表 1
- 无序列表 2
- 无序列表 3
```

如果要控制列表的层级，则需要在符号`-`前使用空格。如下：

- 无序列表 1
- 无序列表 2
  - 无序列表 2.1
  - 无序列表 2.2
``` markdown
- 无序列表 1
- 无序列表 2
  - 无序列表 2.1
  - 无序列表 2.2
```


### 有序列表

有序列表的使用，在数字及符号`.`后加空格后输入内容，如下：

1. 有序列表 1
2. 有序列表 2
3. 有序列表 3

### 引用

引用的格式是在符号`>`后面书写文字。如下：

> 一个人的生命应当是这样度过的：当他回首往事时，不因虚度年华而悔恨，也不因碌碌无为而羞耻。这样，他在临死的时候就能够说：‘我整个的生命和全部精力，都已经献给世界上最壮丽的事业——为人类的解放而作的斗争。 ——奥斯特洛夫斯基

### 链接

中括号中为链接的描述，小括号中为对应的跳转地址

``` markdown
[我的个人介绍](https://pub.yukino.io/about)
```
展示效果为[我的个人介绍](https://pub.yukino.io/about)

### 图片

插入图片，支持 jpg、png、gif、svg 等图片格式。

我们可以在仓库中建立一个专门存储图片的目录，然后利用相对路径来插入图片。

``` markdown
![logo](/images/markdown_logo.jpeg)
```

展示效果为

![logo](/images/markdown_logo.jpeg)

注意在插入图片的时候需要注意图片的格式，如果格式错误则会插入图片失败，例如

``` markdown
![logo](/images/markdown_logo.png)
```

展示效果为

![logo](/images/markdown_logo.png)

直接使用markdown自带的图片插入语法没有办法调整图片的大小，一种最简单粗暴的解决方法是在插入图片前直接调整图片的大小，另一种更加灵活的方式则是使用html代码，这一部分大家可以选择自行掌握，语法如下

``` html
<img src="/images/markdown_logo.jpeg" alt="" width="600" height="300">
```

展示效果为

<img src="/images/markdown_logo.jpeg" alt="" width="600" height="300">

### 分割线

可以在一行中用三个以上的减号来建立一个分隔线，同时需要在分隔线的上面空一行。如下：

---
``` markdown
---
```

### 表格

可以使用冒号来定义表格的对齐方式，如下：

| 姓名   | 年龄 |     工作 |
| :----- | :--: | -------: |
| 小可爱 |  18  | 吃可爱多 |
| 小小勇敢 |  20  | 爬棵勇敢树 |
| 小小小机智 |  22  | 看一本机智书 |
``` markdown
| 姓名   | 年龄 |     工作 |
| :----- | :--: | -------: |
| 小可爱 |  18  | 吃可爱多 |
| 小小勇敢 |  20  | 爬棵勇敢树 |
| 小小小机智 |  22  | 看一本机智书 |
```



## 特殊语法

### 脚注

这是一个例句[^1]。

[^1]: 这是脚注的内容。

``` markdown
这是一个例句[^1]。

[^1]: 这是脚注的内容。
```

脚注内容请拉到最下面观看。

### 代码块

如果在一个行内需要引用代码，只要用反引号引起来就好，如下：

Use the `printf()` function.
``` markdown
Use the `printf()` function.
```

在需要高亮的代码块的前一行及后一行使用三个反引号，同时**第一行反引号后面表示代码块所使用的语言**，如下：

``` stata
// FileName: regression.do
regress y x
```

### 数学公式

行内公式使用方法，比如这个数学公式：$e^{i\pi}+1=0$.

块公式使用方法如下：

$$f_{X}(x)=\frac{1}{\sqrt{2\pi}}\cdot\exp\left(-\frac{x^2}{2}\right),\quad x\in\mathbb{R}.$$

矩阵：

$$
  \begin{pmatrix}
  1 & a_1 & a_1^2 & \cdots & a_1^n \\
  1 & a_2 & a_2^2 & \cdots & a_2^n \\
  \vdots & \vdots & \vdots & \ddots & \vdots \\
  1 & a_m & a_m^2 & \cdots & a_m^n \\
  \end{pmatrix}
$$

``` markdown

$$f_{X}(x)=\frac{1}{\sqrt{2\pi}}\cdot\exp\left(-\frac{x^2}{2}\right),\quad x\in\mathbb{R}.$$

$$
  \begin{pmatrix}
  1 & a_1 & a_1^2 & \cdots & a_1^n \\
  1 & a_2 & a_2^2 & \cdots & a_2^n \\
  \vdots & \vdots & \vdots & \ddots & \vdots \\
  1 & a_m & a_m^2 & \cdots & a_m^n \\
  \end{pmatrix}
$$
```
