---
layout: post
title: Markdown 基本语法
category: interview
tags: [markdown]
---
Markdown是一种纯文本格式的标记语言，Markdown的目标是实现**易读易写**。

## [#](#section) <span id="section">目录</span>
1. [标题](#section1)
2. [段落跟换行](#section2)
3. [列表](#section3)
4. [区块引用](#section4)
5. [分割线](#section5)
6. [表格](#section6)
7. [代码](#section7)
8. [流程图](#section8)
9. [链接](#section9)
10. [强调](#section10)
11. [图片](#section11)

## [#](#section1) <span id="section1">标题</span>

基本语法：

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

效果：

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

## [#](#section2) <span id="section2">段落跟换行</span>

一个Markdown段落是由一个或者多个连续的文本组成，它的前后要有一个以上的空行（空行的定义是显示上看起来是空的，便会被视为空行。例如某行只包含一个空格或者一个制表符，则该行会被视为空行）。

在Markdown段落中插入一个换行符，并不会转行成`<br />`标签，来显示换行，而是显示为空格。如果你想在Markdown中插入`<br />`标签来换行，要在插入处先按入两个以上的空格然后回车。

ps: 在Markdown中需要利用多加空格来产生`<br />`标签，但是利用Markdown中的多段落的列表进行排版，更加好用，更加方便阅读。

## [#](#section3) <span id="section3">列表</span>

Markdown支持无序列表和有序列表。

无序列表基础语法：

```
* Red
* Blue

+ Red
+ Blue

- Red
- Blue

注意：- + * 跟内容之间都要有一个空格
```

效果：

* Red
* Blue
+ Red
+ Blue
- Red
- Blue

有序列表基础语法：

```
1. Bird
2. McHale
3. Parish
注意：数字. 跟内容之间都要有一个空格
```
效果：

1. Bird
2. McHale
3. Parish

## [#](#section4) <span id="section4">区块引用</span>

区块引用基础语法：

```
> This is a blockquote with two paragraphs.
```

效果：

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

## [#](#section5) <span id="section5">分割线</span>

三个或者三个以上的-或者*都可以。

分割线基础语法：

```
---
----
***
*****
```

效果：

---
----
***
*****

## [#](#section6) <span id="section6">表格</span>

表格基础语法：

```
表头|表头|表头
---|:--:|---:
内容|居中内容|居右内容
内容|内容|内容

语法解释：
第二行分割表头跟内容。- 有一个就好。
文字默认居左；
- 两边加 : 表示文字居中；
- 右边加 : 表示文字居右；
```

效果：

表头|表头|表头
---|:--:|---:
内容|居中内容|居右内容
内容|内容|内容

## [#](#section7) <span id="section7">代码</span>

代码基础语法：

```
单行代码  `单行代码内容`

多行代码  ```
            多行代码
            内容
         ```
```

效果：

`单行代码内容`   

```
多行代码
内容
```

## [#](#section8) <span id="section8">流程图</span>

流程图基础语法：

```
```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```
```

暂不支持流程图。

## [#](#section9) <span id="section9">链接</span>

基础语法：

```
[首页](/diary/ '首页')
```

效果：

[首页](/diary/ '首页')

## [#](#section10) <span id="section10">强调</span>

基础语法：

```
**这是加粗的文字**
__这是倾斜的文字__

*这是倾斜的文字*
_这是倾斜的文字_

***这是斜体加粗的文字***

~~这是加删除线的文字~~
```

效果：

**这是加粗的文字**

__这是加粗的文字__

*这是倾斜的文字*

_这是倾斜的文字_

***这是斜体加粗的文字***

~~这是加删除线的文字~~

## [#](#section11) <span id="section11">图片</span>

基础语法：

```
![Alt text](/path/to/img.jpg "Optional title")
```

效果：

![LOGO]({{ '/assets/images/favicon.ico' | prepend: site.baseurl | replace: '//', '/' }} "LOGO")


**附录：** [Markdown官方文档传送门](https://www.appinn.com/markdown/)
