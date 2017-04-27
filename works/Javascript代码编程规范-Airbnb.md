---
title: Javascript代码编程规范-Airbnb
tags: [编程规范]
date: 2017-4-4 13:25:22
categories: JavaScript
---



截止到本文发布,Airbub规范在GitHub上已经5万颗星了.

块级作用域
（1）let取代var
ES6提出了两个新的声明变量的命令： let  和 const  。其中， let  完全
可以取代 var  ，因为两者语义相同，而且 let  没有副作用。
```js
'use strict';
if (true) {
let x = 'hello';
}
for (let i = 0; i < 10; i++) {
console.log(i);
}

```
上面代码如果用 var  替代 let  ，实际上就声明了两个全局变量，这显然
不是本意。变量应该只在其声明的代码块内有效， var  命令做不到这一
点。
var  命令存在变量提升效用， let  命令没有这个问题。

```js
'use strict';
if(true) {
console.log(x); // ReferenceError
let x = 'hello';
}
```

上面代码如果使用 var  替代 let  ， console.log  那一行就不会报错，
而是会输出 undefined  ，因为变量声明提升到代码块的头部。这违反了变
量先声明后使用的原则。
所以，建议不再使用 var  命令，而是使用 let  命令取代。
（2）全局常量和线程安全
在 let  和 const  之间，建议优先使用 const  ，尤其是在全局环境，不
应该设置变量，只应设置常量。这符合函数式编程思想，有利于将来的分布
式运算。

```
// bad
var a = 1, b = 2, c = 3;
// good
const a = 1;
const b = 2;
const c = 3;
// best
const [a, b, c] = [1, 2, 3];
```

const  声明常量还有两个好处，一是阅读代码的人立刻会意识到不应该修
改这个值，二是防止了无意间修改变量值所导致的错误。
所有的函数都应该设置为常量。
长远来看，JavaScript可能会有多线程的实现（比如Intel的River Trail那一类的
项目），这时 let  表示的变量，只应出现在单线程运行的代码中，不能是
多线程共享的，这样有利于保证线程安全。
字符串
静态字符串一律使用单引号或反引号，不使用双引号。动态字符串使用反引
号。

```
// bad
const a = "foobar";
const b = 'foo' + a + 'bar';
// acceptable
const c = `foobar`;
// good
const a = 'foobar';
const b = `foo${a}bar`;
const c = 'foobar';
```


解构赋值
使用数组成员对变量赋值时，优先使用解构赋值。

```
const arr = [1, 2, 3, 4];
// bad
const first = arr[0];
const second = arr[1];
// good
const [first, second] = arr;
```

函数的参数如果是对象的成员，优先使用解构赋值。

```
// bad
function getFullName(user) {
const firstName = user.firstName;
const lastName = user.lastName;
}
// good
function getFullName(obj) {
const { firstName, lastName } = obj;
}
// best
function getFullName({ firstName, lastName }) {
}
```

如果函数返回多个值，优先使用对象的解构赋值，而不是数组的解构赋值。
这样便于以后添加返回值，以及更改返回值的顺序。

```
// bad
function processInput(input) {
return [left, right, top, bottom];
}
// good
function processInput(input) {
return { left, right, top, bottom };
}
const { left, right } = processInput(input);
```

对象
单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个
成员以逗号结尾。

```
// bad
const a = { k1: v1, k2: v2, };
const b = {
k1: v1,
k2: v2
};
// good
const a = { k1: v1, k2: v2 };
const b = {
k1: v1,
k2: v2,
};
```

对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可
避免，要使用 Object.assign  方法。

```
// bad
const a = {};
a.x = 3;
// if reshape unavoidable
const a = {};
Object.assign(a, { x: 3 });
// good
const a = { x: null };
a.x = 3;
```

如果对象的属性名是动态的，可以在创造对象的时候，使用属性表达式定
义。

```
// bad
const obj = {
id: 5,
name: 'San Francisco',
};
obj[getKey('enabled')] = true;
// good
const obj = {
id: 5,
name: 'San Francisco',

[getKey('enabled')]: true,
};
```

上面代码中，对象 obj  的最后一个属性名，需要计算得到。这时最好采用
属性表达式，在新建 obj  的时候，将该属性与其他属性定义在一起。这样
一来，所有属性就在一个地方定义了。
另外，对象的属性和方法，尽量采用简洁表达法，这样易于描述和书写。

```
var ref = 'some value';
// bad
const atom = {
ref: ref,
value: 1,
addValue: function (value) {
return atom.value + value;
},
};
// good
const atom = {
ref,
value: 1,
addValue(value) {
return atom.value + value;
},
};
```

数组
使用扩展运算符（...）拷贝数组。

```
// bad
const len = items.length;
const itemsCopy = [];
let i;
for (i = 0; i < len; i++) {
itemsCopy[i] = items[i];
}
// good
const itemsCopy = [...items];
```

使用Array.from方法，将类似数组的对象转为数组。

```
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

函数
立即执行函数可以写成箭头函数的形式。

```
(() => {
console.log('Welcome to the Internet.');
})();
```

那些需要使用函数表达式的场合，尽量用箭头函数代替。因为这样更简洁，
而且绑定了this。

```
// bad
[1, 2, 3].map(function (x) {
return x * x;
});
// good
[1, 2, 3].map((x) => {
return x * x;
});
// best
[1, 2, 3].map(x => x * x);
```

箭头函数取代 Function.prototype.bind  ，不应再用self/_this/that绑定
this。

```
// bad
const self = this;
const boundMethod = function(...params) {
return method.apply(self, params);
}
// acceptable
const boundMethod = method.bind(this);
// best
const boundMethod = (...params) => method.apply(this,
params);
```

简单的、单行的、不会复用的函数，建议采用箭头函数。如果函数体较为复
杂，行数较多，还是应该采用传统的函数写法。
所有配置项都应该集中在一个对象，放在最后一个参数，布尔值不可以直接
作为参数。

```
// bad
function divide(a, b, option = false ) {
}
// good
function divide(a, b, { option = false } = {}) {
}
```

不要在函数体内使用arguments变量，使用rest运算符（...）代替。因为rest运
算符显式表明你想要获取参数，而且arguments是一个类似数组的对象，而
rest运算符可以提供一个真正的数组。

```
// bad
function concatenateAll( ) {
const args = Array.prototype.slice.call(arguments);
return args.join('');
}
// good
function concatenateAll(...args) {
return args.join('');
}
```

使用默认值语法设置函数参数的默认值。

```
// bad
function handleThings(opts) {
opts = opts || {};
}
// good
function handleThings(opts = {}) {
// ...
```

}
Map结构
注意区分Object和Map，只有模拟现实世界的实体对象时，才使用Object。如
果只是需要 key: value  的数据结构，使用Map结构。因为Map有内建的遍
历机制。

```
let map = new Map(arr);
for (let key of map.keys()) {
console.log(key);
}
for (let value of map.values()) {
console.log(value);
}
for (let item of map.entries()) {
console.log(item[0], item[1]);
}
```


Class
总是用Class，取代需要prototype的操作。因为Class的写法更简洁，更易于理
解。

```
// bad
function Queue(contents = []) {
this._queue = [...contents];
}
Queue.prototype.pop = function( ) {
const value = this._queue[0];
this._queue.splice(0, 1);
return value;
}
// good
class Queue {
constructor(contents = []) {
this._queue = [...contents];
}
pop() {
const value = this._queue[0];
this._queue.splice(0, 1);
return value;
}
}
```

使用 extends  实现继承，因为这样更简单，不会有破坏 instanceof  运
算的危险。

```
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function( ) {
return this._queue[0];
}
// good

class PeekableQueue extends Queue {
peek() {
return this._queue[0];
}
}
```


模块
首先，Module语法是JavaScript模块的标准写法，坚持使用这种写法。使
用 import  取代 require  。

```
// bad
const moduleA = require('moduleA');
const func1 = moduleA.func1;
const func2 = moduleA.func2;
// good
import { func1, func2 } from 'moduleA';
使用 export  取代 module.exports  。
```


```
// commonJS的写法
var React = require('react');
var Breadcrumbs = React.createClass({
render() {
return <nav />;
}
});
module.exports = Breadcrumbs;
// ES6的写法
import React from 'react';
const Breadcrumbs = React.createClass({
render() {
return <nav />;
}
});
export default Breadcrumbs
```

如果模块只有一个输出值，就使用 export default  ，如果模块有多个输
出值，就不使用 export default  ，不要 export default  与普通
的 export  同时使用。

不要在模块输入中使用通配符。因为这样可以确保你的模块之中，有一个默
认输出（export default）。

```
// bad
import * as myObject './importModule';
// good
import myObject from './importModule';
```

如果模块默认输出一个函数，函数名的首字母应该小写。

```
function makeStyleGuide( ) {
}
export default makeStyleGuide;
```

如果模块默认输出一个对象，对象名的首字母应该大写。

```
const StyleGuide = {
es6: {
}
};
export default StyleGuide;
```






