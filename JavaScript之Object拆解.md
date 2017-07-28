---
title: JavaScript之Object拆解
tags:
  - Object
categories: JavaScript
date: 2017-07-26 19:17:56
---

JavaScript是基于对象的语言, String、Array等本质来说都是 Object 的实例, 原型链也会指向 Object.

```js
new Array() instanceof Object // true
new String() instanceof Object // true
new Function() instanceof Object // true
```

下面我们一起来拆解下 Object 的方法和新 Api:

### 类型判断




### Object原型上的方法





### ES6 之后的新特性

#### 对象属性的简洁写法

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

#### 字面量形式定义对象

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


#### Object.is('','') 同值相等API

`Object.is()` 基本等同于 `===` , 除却两点:

```js
+0 === -0 // true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```
#### Object.assign(target, source1, source2) 复制可枚举属性

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

#### 属性的可枚举性

每个对象属性都有一个描述对象 Description , 可以控制是否可被枚举.

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

#### 属性的遍历

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

#### __proto__, Object.getPrototypeOf(), Object.setPrototypeOf()

##### __proto__

__proto__ 指向当前实例的原型对象, 其没有被 ES6 列为正式 API, 但因为被浏览器厂商广泛使用, 被收入附录.

某些浏览器厂商同样指向原型对象, 可能是另一种命名方式, 所以为了兼容性考虑, 最好不要通过它去操作原型.

##### Object.setPrototypeOf()

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
##### Object.getPrototypeOf()

Object.getPrototypeOf(obj) 是 ES6 返回原型对象的方法

#### Object.keys(), Object.values(), Object.entries()

Object.keys() 是 ES5 中遍历属性的方法, ES6 新增了 Object.values(), Object.entries().

- Object.values

返回对象自身的(不包含继承的), 可枚举的键值

- Object.entries()

返回对象自身的(不包含继承的), 可枚举的键值对数组

#### 对象的拓展运算符

ES8 中将数组的拓展运算符引入到了对象中.

##### 解构

```js
let {a, b, ...x} = {a: 1, b:2, c: 3, d: 4};
console.log(x);
// {c:3,d:4}
```
注意:

1. 拓展运算符会复制对象所有未读取的键值对, 所以右侧必须是对象或可转换为对象, 不能为 null 或 undefined
2. 拓展运算符必须置为最后一个位置, 否则会报错
3. 拓展运算符不会复制原型上的方法

##### 克隆对象

```js
let x = {name: 'XingChuan', age: 88};
let cloneObj = { ...x };
```

##### 合并对象

```js
let x = {name: 'XingChuan', age: 88};
let y = {job: 'developer'};
let cloneObj = { ...x, ...y };
```
##### 拓展运算符表达式

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

#### Object.getOwnPropertyDescriptors()

ES5 中 Object.getOwnPropertyDescriptor(obj, property) 可以获取对象属性的描述对象.

ES8 中 新增了 Object.getOwnPropertyDescriptors(obj) 可以获取对象所有属性的描述对象, 描述对象包括 get 和 set 属性.






