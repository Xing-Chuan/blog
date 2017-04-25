---
title: IE浏览器兼容性问题
tags:
  - IE
  - 兼容性
categories: works
date: 2016-04-25 18:31:07
---

- 媒体查询 ( Media Query )

IE8 无法识别 Media Query , 推荐用 Respond.js 解决

- HMTL5 新标签

nav, footer 等新标签, 推荐用 html5shiv.js 解决

- max-width

IE8中经常遇到的问题就是 max-width，网页中图片的尺寸可能比较宽，我会给它设置 max-width: 100%来限制其宽度最大为父容器的宽度，但是有时候却不奏效，慢慢摸索才得知IE解析max-width所遵循的规则：
严格要求直接父元素的宽度是固定的, 实验发现 Chrome 所遵守的规则比IE松一些.

（1）td中的max-width

如果针对td中的img元素设置max-width: 100%，在IE和Firefox你会发现不奏效，而在Chrome中却是可以的。经查询发现需要给table设置table-layout: fixed，对此属性的具体解释见W3School。

（2）嵌套标签中的max-width

如下的HTML结构：
```html
<div class="work-item">
    <a href="#" class="work-link">
        <img src="sample.jpg" class="work-image img-responsive">
    </a>
</div>
```
最外层元素.work-item设置了固定宽度，但是对img设置max-width为100%却无效，后来才发现需要再对a标签设置width: 100%，这样才能使最内层的img标签充满整个div。

- 嵌套inline-block下padding元素重叠

HTML代码：
```html
<ul>
    <li><a>1</a></li>
    <li><a>2</a></li>
    <li><a>3</a></li>
</ul>
```
CSS代码：
```css
ul li{
    display: inline-block;
}
ul li a{
    display: inline-block;
    padding: 10px 15px;
}
```
按理来说a标签之间的距离应该是30px，但在IE8中出现了重叠，只有15px。这里和这里也提到了同样的问题。我的解决方法是使用float: left替代display: inline-block实现水平布局。

- placeholder

IE8下不支持HTML5属性placeholder，不过为解决此问题的js插件挺多的，比如：jquery-placeholder。

- last-child

first-child是CSS2的内容，但是last-child就不是了，所以IE8不买账。推荐的做法不是使用last-child，而是给最后一个元素设置一个.last的class，然后对此进行样式设置，这样就全部兼容了。

- background-size: cover

如果你想使用background-size: cover设置全屏背景，很遗憾IE8办不到...但可以使用IE独有的AlphaImageLoader滤镜来实现，添加一条如下的CSS样式：

filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(enabled=Enabled, sizingMethod=Size , src=URL)
将sizingMethod设置为scale就OK了。

还没完，如果你在此背景之上放置了链接，那这个链接是无法点击的。一般情况下的解决办法是为链接或按钮添加position:relative使其相对浮动。

- console.log() 打断 IE8 Ajax请求语句

慎用

- 块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大

在float的标签样式控制中加入 display:inline;将其转化为行内属性

- 设置较小高度标签（一般小于10px），在IE6，IE7，遨游中高度超出自己设置高度

给超出高度的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。

- 行内属性标签，设置display:block后采用float布局，又有横行的margin的情况，IE6间距bug

在display:block;后面加入display:inline;display:table;

- 图片默认有间距

使用float属性为img布局

- 标签最低高度设置min-height不兼容

如果我们要设置一个标签的最小高度200px，需要进行的设置为：{min-height:200px; height:auto !important; height:200px; overflow:visible;}

- 透明度的兼容CSS设置

ie中用的是filter:alpha(opacity=0);这个属性来设置div或者是块级元素的透明度，而在firefox中，一般就是直接使用opacity:0,对于兼容的，一般的做法就是在书写css样式的将2个都写上就行


