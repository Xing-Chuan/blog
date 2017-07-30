---
title: JavaScript之Object拆解
tags:
  - Object
categories: JavaScript
date: 2017-07-26 19:17:56
---

JavaScript是一门基于对象的语言, 关于基于对象和面向对象的差异, 我曾看到这样的比喻:

- 基于对象，就是一个工程师建了一栋房子，然后其它的工程师按照这个房子的样子去建造其它的房子
- 面向对象，就是一个工程师再图纸上设计出一栋房子的样子，然后其它工程师按照这个图纸的设计去建造房子

也就是说:

- 基于对象是先有一个具体的对象，然后在这个对象的基础上创建新的对象
- 面向对象就是先有一个抽象的对象描述，然后以此为蓝本构建具体对象

JavaScript 中这个具体的对象就是 Object.
下面, 让我们一起来拆解下 Object 相关联的属性、方法和 ES6 后新 Api 吧:

## 属性类型

JavaScript 中有两种数据类型: 数据属性和访问器属性

### 数据属性

数据属性有以下几个描述行为的属性:

- Configurable 描述这个属性是否可被 delete 删除, 默认为 true
- Enumerable 描述这个属性是否可被枚举, 默认为 true
- writable 描述这个属性是否可被修改, 默认为 true
- value 描述这个属性的值

如果想要修改这些系统默认属性, 可以通过 ES5 的方法 Object.defineProperty(obj, property, option).

注意:

- 可以多次调用 Object.defineProperty() 修改属性, 但将 Configurable 设置为 false 是不可逆的.
- 调用 Object.defineProperty() 修改属性方法时, 如果不指定这些描述属性, 则默认值都会设置为 false.

### 访问器属性

访问器有以下几个属性:

- Configurable 描述这个属性是否可被 delete 删除, 默认为 true
- Enumerable 描述这个属性是否可被枚举, 默认为 true
- get 在读取属性时调用的函数, 默认为 undefined
- set 在设置属性时调用的函数, 默认为 undefined

```js
let book = { _year: 2004, edition: 1 };

/*
 * 第一个参数: 对象
 * 第二个参数: 对象的属性
 * 第三个参数: 需要设置的描述属性
 */

Object.defineProperty(book, 'year', {
  get: function() { return this._year; },
  set: function(newValue) {
    if (newValue > 2004) {
      this._year = newValue;
      this.edition += newValue - 2004;
    }
  }
});

book.year = 2009;
console.log(book.edition); // 6
```

注意:

- get 和 set 必须同时设置, 否则会出现不能读或不能写的情况

如果要一次修改多个参数的描述属性, 可以使用 Object.defineProperties()

```js
let book = {};

Object.defineProperties(book, {
  _year: {
    value: 2004
  },
  edition: {
    value: 1
  }
});
```
## 构造函数

构造函数也是普通的函数, 如果没有通过 new 操作符生成新的实例对象, 那构造函数就是一个普通的函数.
为了区分构造函数于普通函数, 我们通常用大驼峰命名法给构造函数命名.
String、Array 等几乎所有对象都是 Object 的实例, Object 就是一个构造函数.

```js
new Array() instanceof Object // true
new String() instanceof Object // true
new Function() instanceof Object // true
```

```js
function Info() {
  this.name = 'XingChuan';
  this.age = '10';
}

let info1 = new Info();
let info2 = new Info();
console.log(info1.name); // XingChuan
console.log(info1.age); // 10
console.log(info2.name); // XingChuans
console.log(info2.age); // 10
```

## 原型对象

既然谈到构造函数就要谈到原型了, 每个函数都有一个原型(prototype)属性, 原型存在的意义就是, 原型上的属性和方法由所有构造函数的实例对象所共享, 如果修改了原型对象上的方法, 那所有实例都会实时更改.

我们平时使用 Array 和 String 的方法, 都是在其原型对象上, 所有我们创建的所有数组和字符串都可以享有这些方法.

![image.png](http://upload-images.jianshu.io/upload_images/4434201-b332ef5ec41e1a0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 原型链

在 chrome, firefox, safari 中, 有一个浏览器私有属性 `__proto__` 指向构造函数的 prototype, `__proto__` 并不是官方属性, 为了兼容性考虑, 开发中最好不要使用 `__proto__`.

下面我们来具象化一下原型链的构成:

### 函数的原型链

构造函数也是普通的函数, 只是我们拿来生成实例, 所以才有这个称谓, 而所有的函数都是 Function 的实例.

```js
function info() {

}
```

```bash
// 原型链
info.__proto__ => Function.prototype => Object.prototype
```

![image.png](http://upload-images.jianshu.io/upload_images/4434201-3315b5f26f6ff6ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所有的函数都是 Function 的实例, 而 Function 是 Object 的实例, 所以有了这条原型链.

### 内置对象的原型链

很有意思的是, Object、Array、String、Function 都是函数, 所以他们都是 Function 的实例, Function 比较特殊, 它也是自身的实例.

Math 是个例外, 它并不是一个函数, 而是一个对象.

```js
console.log(Array instanceof Function) // true
console.log(String instanceof Function) // true
console.log(Object instanceof Function) // true
console.log(Function instanceof Function) // true
console.log(Math instanceof Function) // false
console.log(Math.__proto__ === Object.prototype); // true
```

```bash
// 原型链
Array.__proto__ => Function.prototype => Object.protytype
```

### 实例对象的原型链

```js
function Info() {
}
Info.prototype.name = 'Xingchuan';
Info.prototype.age = 10;
Info.prototype.showName = function() {
  console.log(this.name);
};
let info1 = new Info();
info1.showName() // XingChuan
let info2 = new Info();
info2.name = 'test';
info2.showName() // test
```
```bash
// 原型链
info1.__proto__ => Info.prototype => Object.prototype
```
实例对象的 `__proto__` 会指向构造函数的 prototype, 所有的原型对象都是 Object.prototype 的实例, 所以构成了这一条原型链.

注意:

- 原型链的访问是有就近原则的, 如果像上面实例中已经有 name 属性, 则不会继续访问 prototype 上的 name 属性, 如果实例中没有这个属性, 则会按照原型链一直找下去, 如果没有就是 undefined.

### 元素也是 Object 的实例

```js
console.dir(document.getElementsByTagName('span')[0]);
// span元素 => HTMLSpanElement => HTMLElement => Element => Node => EventTarget => Object.prototype
console.dir(document.getElementsByTagName('span')[0] instanceof Object);
// true
```

元素的原型链很长, 不过可以看到元素也是 Object 的实例.

### 小结

- 原型链有就近原则, 如果在实例上访问到某属性, 就不会继续原型链寻找该属性了
- 原型是可以重新指定的
- `prototype` 的 constructor 指向 构造函数
- 实例的 `__proto__` 指向构造函数的 `prototype`
- 所有函数的 `__proto__` 指向 `Function.prototype`
- 函数的 `prototype` 都是基于 `Object.prototype`
- Function 也是自己的实例
- 一切起源于 `Object.prototype`


来张示意图结尾( 侵删 ):

![image.png](http://upload-images.jianshu.io/upload_images/4434201-847fdc2301562836.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## this 的指向

<JavaScript忍者禁术> 一书中对 this 指向的问题有比较详细的描写, 推荐看看.

在讨论 this 指向的问题之前, 先要明确一下, 在严格模式下, 未指定环境对象而调用函数，则 this 值不会转型为 window.
除非明确把函数添加到某个对象或者调用 apply()或 call()，否则 this 值将是 undefined.

1. 普通函数中的 this

```js
let x = 1;
function show() {
  console.log(this.x);
}
show(); // 1
```
普通函数中的 this 指向 window

2. 构造函数中的 this

```js
function Info(){
　　this.x = 1;
}
let info1 = new Info();
console.log(info1.x); // 1
```
构造函数中的 this 指向 new 出来的实例对象


3. 对象方法中的 this

```js
let x = 2;
let obj = {
  x: 1,
  y: function() {
    console.log(this.x);
  }
};
obj.y(); // 1

对象方法中的 this 指向调用它的对象.
```

4. call、apply、bind(ES5) 中的 this

call、apply、bind(ES5) 的作用就是改变 this 的指向, this 会指向第一个参数.

如果 call、apply、bind(ES5) 调用方法时没有传参, 默认 this 指向 window.

bind 只会改变 this 的指向, 并不会执行方法, call 和 apply 则会改变指向时也执行方法.

5. 事件函数中的 this

事件函数中的 this 指向绑定事件的元素.

6. 箭头函数中的 this

箭头函数是 ES6 中新增的方法, 不同于其他情况中的 this 在调用时才决定指向, 箭头函数 this 的指向与其身处的环境有关, 一般是指向身处的对象, 如果身处在如定时函数中, this 的指向就是 window 了.



## 数据类型判断

在开发中, 我们可以用 typeof 来判断类型, 但有很大的局限性.

```js
typeof 1
// "number"
typeof '1'
// "string"
typeof true
// "boolean"
typeof undefined
// "undefined"
typeof null
// "object"
typeof (function(){})
// "function"
typeof []
// "object"
typeof {}
// "object"
```
typeof 只对一些简单数据类型有效, 为了可以判断各种内置对象, 我们需要采取一些'手段', 使用 Object 原型上的 toString 方法.

```js
Object.prototype.toString.call(1);
// "[object Number]"
Object.prototype.toString.call('1');
// "[object String]"
Object.prototype.toString.call(true);
// "[object Boolean]"
Object.prototype.toString.call(function() {});
// "[object Function]"
Object.prototype.toString.call(null);
// "[object Null]"
Object.prototype.toString.call(undefined);
// "[object Undefined]"
Object.prototype.toString.call(new Date());
// "[object Date]"
Object.prototype.toString.call(Math);
// "[object Math]"
Object.prototype.toString.call([]);
// "[object Array]"
Object.prototype.toString.call({});
// "[object Object]"
```

可以全部搞定了.

## ES6 之后的新特性

### 对象属性的简洁写法

> 简洁的写法可以减少代码量也可以更加优雅, 但代码是给计算机看的, 同时也是给人看的, 容易发生歧义的地方一定要注意.

- 属性的简写

如果属性名与属性值相同, 可以忽略不写.
属性值是字符串时不可简写.

```js
// old
let name = 'XingChuan';
let obj = {
  name: name
};
// new
let name = 'XingChuan';
let obj = {
  name
};
// error
let name = 'XingChuan';
let obj = {
  name:'name'
};
```

- 函数的简写

```js
// old
let obj = {
  show: function() {
    console.log('show');
  }
};
// new
let obj = {
  show() {
    console.log('show');
  }
};
```

### 字面量形式定义对象

ES5只支持这种字面量定义:

```js
let obj = {
  name: 'XingChuan',
  age: 10
};
```

ES6支持这种写法:

```js
let obj ={
  [name]: 'XingChuan',
  ['a' + 'ge']: 10
};
// 作为属性名的表达式会自动 toString() , 应避免使用对象作为表达式, 因为 String({}) === '[object Object]'
```

注意:

- 属性名表达式与简洁表示法不能同时使用, 会报错
- 属性名表达式为对象时属性名会重复的问题


### Object.is('','') 同值相等API

`Object.is()` 基本等同于 `===` , 除却两点:

```js
+0 === -0 // true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```
### Object.assign(target, source1, source2) 复制可枚举属性

```js
let target = {
  info: {
    name: 'name1'
  }
};
let source1 ={
  info: {
    name: 'name2',
    age: 30
  }
};
let source2 ={
  info: {
    name: 'name3'
  }
};

Object.assign(target, source1, source2);
// { info: {name: 'name3'}}
//
// Object.assign 是浅拷贝, 如果属性的值是对象, 就会添加新的引用, 而不是在原有地址上添加属性.
// target会自动转换为对象, 所以不能为 null 或 undefined , 会报错
// source为 null 或 undefined 时, 因为无法转换为对象, 会跳过, 但不会报错
// 若 source 为字符串, 会以数组的形式复制到 target 中
//
```

### 属性的可枚举性

每个对象属性都有一个描述对象 Description , 可以控制是否可被枚举, 数据属性的其中之一.

```js
let obj = {
  name: 'XingChuan'
};
Object.getOwnPropertyDescriptor(obj,'name');

// {
//   configurable: true,
//   enumerable: true, // 如果可枚举为 false , 某些操作会忽略掉这个属性
//   value: "XingChuan",
//   writable: true
// }
```
ES5 中有 3 个属性会忽略 `enumerable` 为 `false` 的属性:

- for...in 循环
- Object.keys()
- JSON.stringify()

ES6 新增的 Object.assign() 也会忽略描述中不可枚举的属性.

数组中的 length 属性不会被 for...in 获取就是因为不可枚举的描述.

```js
Object.getOwnPropertyDescriptor([1,2,3],'length');

// {
//   configurable: false,
//   enumerable: false,
//   value: 3,
//   writable: true
// }
```

另外, ES6 中也规定了 Class 原型上的方法是不可枚举的.

### 属性的遍历

1. for...in

for...in 循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）

2. Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性)

3. Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）

4. Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性

5. Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的所有属性，不管属性名是 Symbol 或字符串，也不管是否可枚举

以上 5 种方法在遍历顺序上, 遵循以下 3 条规则:

- 首先遍历所有属性名为数值的属性，按照数字排序
- 其次遍历所有属性名为字符串的属性，按照生成时间排序
- 最后遍历所有属性名为 Symbol 值的属性，按照生成时间排序

### `__proto__`, Object.getPrototypeOf(), Object.setPrototypeOf()

#### `__proto__`

`__proto__` 指向当前实例的原型对象, 其没有被 ES6 列为正式 API, 但因为被浏览器厂商广泛使用, 被收入附录.

某些浏览器厂商同样指向原型对象, 可能是另一种命名方式, 所以为了兼容性考虑, 最好不要通过它去操作原型.

#### Object.setPrototypeOf()

Object.setPrototypeOf() 是 ES6 设置原型对象的方法

```js
let obj = {
  x: 10
};
let option = {
  x: 20,
  y: 30,
  z: 40
};
Object.setPrototypeOf(obj, option);

obj.x // 10
obj.y // 30
obj.z // 40

// 因原型链访问顺序的优先级, obj.x 为 10 而不是 20, 如 obj 不存在 x 的属性, obj.x 就会为 20.
```
#### Object.getPrototypeOf()

Object.getPrototypeOf(obj) 是 ES6 返回原型对象的方法

### Object.keys(), Object.values(), Object.entries()

Object.keys() 是 ES5 中遍历属性的方法, ES6 新增了 Object.values(), Object.entries().

- Object.values

返回对象自身的(不包含继承的), 可枚举的键值

- Object.entries()

返回对象自身的(不包含继承的), 可枚举的键值对数组

### 对象的拓展运算符

ES8 中将数组的拓展运算符引入到了对象中.

#### 解构

```js
let {a, b, ...x} = {a: 1, b:2, c: 3, d: 4};
console.log(x);
// {c:3,d:4}
```
注意:

1. 拓展运算符会复制对象所有未读取的键值对, 所以右侧必须是对象或可转换为对象, 不能为 null 或 undefined
2. 拓展运算符必须置为最后一个位置, 否则会报错
3. 拓展运算符不会复制原型上的方法

#### 克隆对象

```js
let x = {name: 'XingChuan', age: 88};
let cloneObj = { ...x };
```

#### 合并对象

```js
let x = {name: 'XingChuan', age: 88};
let y = {job: 'developer'};
let cloneObj = { ...x, ...y };
```
#### 拓展运算符表达式

```js
let obj = { ...{x > 1 ? {a: 1} : {} } };
```

扩展运算符的参数对象之中，如果有取值函数get，这个函数是会执行的.

```js
let runtimeError = {
  ...a,
  ...{
    get x() {
      throws new Error('thrown now');
    }
  }
};
```

### Object.getOwnPropertyDescriptors()

ES5 中 Object.getOwnPropertyDescriptor(obj, property) 可以获取对象属性的描述对象.

ES8 中 新增了 Object.getOwnPropertyDescriptors(obj) 可以获取对象所有属性的描述对象, 描述对象包括 get 和 set 属性.

### Null 传导运算符

我们要读取对象的一个属性或调用其方法, 为了不报错, 应该先判断对象是否存在, 然后再读取其属性.

如果我们想读取 obj.info.xingchuan.name, 安全的写法应该是下面这样

```js
let name = obj && obj.info && obj.info.xingchuan && obj.info.xingchuan.name || 'default';
```

现在提案中引入了 Null 传导运算符, 简化了写法, 可以写为下面这种方式.

```js
let name = obj ?. info ?. xingchuan ?. name || 'default';
```


