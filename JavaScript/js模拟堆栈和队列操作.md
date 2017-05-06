---
title: JS模拟堆栈和队列操作
tags:
  - 堆栈
  - 队列
categories: JavaScript
date: 2016-08-03 21:47:26
---

在面向对象的设计模式中, 一般提供了队列(queue)和堆栈(stack)的方法, 而对于 JS , 可以用数组来模拟其操作流程:

### 堆栈

堆栈操作只允许在一端添加、删除操作, 这端叫做栈顶, 另一端叫做栈底, 秉承后进先出的原则.

```js
    const arr = []
    for (let i = 0; i < 5; i++) {
      const temp = i + 1
      arr.push(temp) 
      console.log(`${temp}, 进栈了`)
    }
    console.log('\n')
    const len = arr.length
    for (let i = 0; i < len; i++) {
      console.log(arr.pop() + `, 出栈了`)
    }
```

### 队列

队列操作允许前端插入, 后端取出, 秉承先进先出的原则.

```js
    const arr = []
    for (let i = 0; i < 5; i++) {
      const temp = i + 1
      arr.unshift(temp) 
      console.log(`${temp}, 插入队列`)
    }
    console.log('\n')
    const len = arr.length
    for (let i = 0; i < len; i++) {
      console.log(arr.pop() + `, 出来了`)
    }

```





