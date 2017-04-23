---
title: 移动web开发之viewport
tags:
  - 移动端
date: 2016-08-07 15:15:51
categories: 移动端
---
> 为了移动多终端适配，苹果提出viewport的概念，并在Safari上推行，目前各厂商均已实现适配

## 开启viewport
浏览器默认不开启viewport,如要开启viewport,需要再head标签中加入以下内容
```js
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```
<!--more-->
让viewport的宽度等于设备宽度,不允许设备缩放,防止误触破坏界面效果,默认同时也是最大缩放比限制在1.0

emmet语法支持meta:vp+tab快捷输出

### viewport属性
在苹果的规范中，meta viewport 有6个属性，如下：
* width 
> 设置layout viewport的宽度，为一个正整数，或设置字符串"width-device",让其等于设备宽度,实现自适应
* initial-scale
> 设置页面的初始缩放值，为一个数字，可以带小数
* minimum-scale
> 允许用户的最小缩放值，为一个数字，可以带小数
* maximum-scale
> 允许用户的最大缩放值，为一个数字，可以带小数
* height
> 设置layout viewport的高度，很少使用
* user-scalable
> 是否允许用户进行缩放,no代表不允许,yes代表允许

这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。
