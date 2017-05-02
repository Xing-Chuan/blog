---
title: JS事件的三个阶段与事件委托的原理
tags:
  - 事件阶段
  - 事件委托
categories: JavaScript
date: 2017-05-02 21:00:18
---

### 事件阶段

DOM2级事件规定的事件流包括三个阶段，分别是：事件捕获阶段，处于目标阶段和事件冒泡阶段.

- 事件冒泡

事件开始时由最具体的元素(文档中嵌套最深的那个节点)接收，然后逐级向上（一直到文档）

```html
<div id = "div">
    <span id="span">
        <a id="aTag">事件测试</a>
    </span>
</div>
```

```js
document.getElementById("aTag").addEventListener('click',aTag);
document.getElementById("span").addEventListener('click',span);
document.getElementById("div").addEventListener('click',div);
function aTag(e) {
    alert("点击的是a标签");
}
function span(e) {
    alert("点击的是span标签");
}
function div(e) {
    alert("点击的是div标签");
}
```

```html
1)先打印出：点击的是a标签

2) 再打印出：点击的是span标签

3) 最后打印出：点击的是div标签
```


- 事件捕获

事件捕获与事件冒泡事件流正好相反的顺序，事件捕获的事件流是最外层逐级向内传播，也就是先document，然后逐级div标签 ， span标签 , a标签.

改写上面的 js 代码:

```js
document.getElementById("div").addEventListener('click',div,true);
document.getElementById("aTag").addEventListener('click',aTag,true);
document.getElementById("span").addEventListener('click',span,true);
```

第三个参数设置为 true 为捕获事件, 默认为 false 是冒泡事件


### 事件委托

事件委托可以避免对每个节点添加事件监听, 我们可以利用冒泡原理, 将事件绑定到父元素上.

以下需要对父元素做事件委托:

1. 很多兄弟元素需要绑定同一个事件时
2. 子元素需要动态插入或删除时

#### 事件委托的原理

当我们进行事件委托时, 事件监听器会分析从子元素冒泡上来的事件，找到是哪个子元素的事件.

当我们有如下文档结构时:

```html
<ul id="parent-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

当子元素的事件冒泡到父 ul 元素时，你可以检查事件对象的 target 属性，捕获真正被点击的节点元素的引用。下面是一段 JavaScript 代码，演示了事件委托的过程：

```js
document.getElementById("parent-list").addEventListener('click', function (e) {
    if (e.target && e.target.nodeName == 'LI') {
      console.log(`List item,${e.target.id}, was clicked!`)
    }
  })
```

执行过程:

1. 给父元素添加事件监听器
2. 事件触发时, 检查事件来源
3. 如果触发事件的是 li 元素, 执行对应代码, 否则不执行

