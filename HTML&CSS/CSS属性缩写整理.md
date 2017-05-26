---
title: CSS属性缩写整理
tags:
  - 属性缩写
categories: CSS
date: 2017-05-26 09:56:01
---

属性缩写可以提高代码编辑效率, 也可以减少冗余代码, 是开发过程中不可或缺的部分.

### border-width

```css
border-width: 1px; // 上右下左均1px
border-width: 1px 2px; // 上下1px 左右2px
border-width: 1px 2px 3px; // 上1px 左右2px 下3px
border-width: 1px 2px 3px 4px; // 上1px 右2px 下3px 左1px
```

### border-radius

```css
border-radius: 1px; // 四个角均1px
border-radius: 1px 2px; // 上左、下右1px 上右、下左2px
border-radius: 1px 2px 3px; // 上左1px 上右、下左2px 下右3px
border-radius: 1px 2px 3px 4px; // 上左1px 上右2px 下右3px 下左1px
```

### background

```css
background-color: #000;
background-image: url(images/bg.gif);
background-repeat: no-repeat;
background-position: top right;
```

简写:
```css
background: #000 url(images/bg.gif) no-repeat top right;
```

### font

```css
font-style: italic;
font-weight: bold;
font-size: .8em;
line-height: 1.2;
font-family: Arial, sans-serif;
```

简写:
```css
font: italic bold .8em/1.2 Arial, sans-serif;
```

### border

```css
border-width: 1px;
border-style: solid;
border-color: #000;
```

简写:
```css
border: 1px solid #000;
```

### margin&padding

```css
margin-top: 10px;
margin-right: 5px;
margin-bottom: 10px;
margin-left: 5px;
```

简写:
```css
margin: 10px 5px 10px 5px;
```

这里主要借鉴 MDN 上的信息, 感觉总结的不是很全面, CSS3 的属性都没有体现, 等有时间再扩充下 CSS3 的属性缩写部分.









