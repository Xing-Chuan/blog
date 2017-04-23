---
title: rem布局 body元素font-size初始化的必要性
tags:
  - CSS
  - Bug
date: 2016-02-13 16:11:18
categories: CSS
---
> 在我们进行rem布局时,通常给`body`设置`font-size:14px`或`font-size:16px`来进行初始化,今天来深究一下这是why?

## 什么是rem布局?
我们通过给`html`元素设置`font-size`,并给元素设置rem单位,来响应不同手机端的缩放.

## body元素font-size初始化的必要性
`font-size`是可以被后代元素继承的属性,如果不给`body`设置`font-size`值,`html`下后代元素都会继承`html`的`font-size`值.
默认设置一般为`100px`,画风是不是有点奇怪呢.
## font-size除了影响文字大小,还会影响什么?
> 今天的重头戏来了,除了影响文字大小,还会影响什么呢?

布局中我们常注意到图片下多出的**几像素**,这个问题老生常谈了,原因我们已经知道,
图片是默认对应`baseline`的.

前几日,有人找我review代码,有个问题:"input参数设置正确,为什么头顶还有一大片空白呢?"

画风是这样的:

![](/img/font-size01.png)

<!--more-->

**input元素未受到padding和margin影响,此时是初始位置**

经过排查,是因为`body`没设置`font-size`,`form`元素继承`html`的字体大小了,那么问题来了,
为什么`font-size`会对`input`元素产生影响?

大家应该想到之前`img`元素下方留白的问题了,其原理是一样的,**行内元素**都是沿`font`的`baseline`对齐的

如果`font-size`设置的过大,`baseline`就会下移,即使没有文字,行内元素也是会根据`baseline`走的,自然就出现位移的问题.

## 如何解决font-size对行内元素的影响?
> 我们这里讨论不止限于rem布局的方法

我们的目标是达到下图的效果:

![](/img/font-size02.png)

1. 行内元素转成块元素
```css
  /*块元素自占一行*/
  display:block;
```
2. 浮动
```css
  /*脱离标准流*/
  float:left;
  float:right;
```
3. 定位
```css
  /*脱离标准流*/
  position:absolute;
  position:relative;
```
4. vertical-align
```css
  /*直接改变对齐方式*/
  vertical-align:top;
```

以上几种方法都可以解决这个问题