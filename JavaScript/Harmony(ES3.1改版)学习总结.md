---
title: Harmony(ES3.1改版)学习总结
tags: [Harmony,ES3.1]
date: 2016-6-10 09:53:44
categories: JavaScript
---

## 变量声明
- let
> 只在当前块级作用域中有效

let语句:只能在后续代码块中使用的变量

```Javascript
var num = 5;
let (num=10, multiplier=2){
alert(num * multiplier); //20
}
alert(num); //5
```
let 表达式:

```
var result = let(num=10, multiplier=2) num * multiplier;
alert(result); //20
//这里的 let 表达式使用两个变量计算后得到一个值，保存在变量 result中。执行表达式之后， num和 multiplier 变量就不存在了。
```

- const
> 常量,声明后不能再更改其中的值

## 函数
### 剩余参数与分布参数
- 剩余参数 ( rest arguments ) 

剩余参数的语法形式是三个点后跟一个标识符`...nums`


```Javascript
function sum(num1, num2, ...nums){
var result = num1 + num2;
for (let i=0, len=nums.length; i < len; i++){
result += nums[i];
}
return result;
}
var result = sum(1, 2, 3, 4, 5, 6);
console.log(result) //21
```
- 分布参数（spread arguments）

通过分布参数，可以向函数中传入一个数组，然后数组中的元素会映射到函数的每个参数上

分布参数的语法形式与剩余参数的语法相同，就是在值的前面加三个点。唯一的区别是分布参数在调用函数的时候使用，而剩余参数在定义函数的时候使用

```
var result = sum(...[1, 2, 3, 4, 5, 6]);
```
```
var result = sum.apply(null, [1, 2, 3, 4, 5, 6]);
```

#### 参数默认值
要为参数指定默认值，可以在参数名后面直接加上等于号和默认值
```Javascript
function sum(num1, num2=0){
return num1 + num2;
}
var result1 = sum(5);
var result2 = sum(5, 5);
```

#### 生成器

对 Harmony 而言，要创建生成
器，可以让函数通过 yield 操作符返回某个特殊的值。对于使用 yield操作符返回值的函数，调用它时就会创建并返回一个新的 Generator 实例。然后，在这个实例上调用 next() 方法就能取得生成器的第一个值。此时，执行的是原来的函数，但执行流到 yield 语句就会停止，只返回特定的值。从这个角度看， yield 与 return 很相似。如果再次调用 next() 方法，原来函数中位于 yield 语句后的代码会继续执行，直到再次遇见 yield 语句时停止执行，此时再返回一个新值

```Javascript
function myNumbers(){
for (var i=0; i < 10; i++){
yield i * 2;
}
}
var generator = myNumbers();
try {
while(true){
document.write(generator.next() + "<br />");
}
} catch(ex){
//有意没有写代码
} finally {
generator.close();
}
```

## 数组及其他结构
### 迭代器
要为对象创建迭代器，可以调用 Iterator 构造函数，传入想要迭代其值的对象。要取得对象中的
下一个值，可以调用迭代器的 next() 方法
```
var person = {
name: "Nicholas",
age: 29
};
var iterator = new Iterator(person);
try {
    while(true){
    let value = iterator.next();
    document.write(value.join(":") + "<br>");
}
} catch(ex){
//有意没有写代码
}
```

### 数组领悟
JavaScript 中数组领悟的基本形式如下：
```
array = [ value for each (variable in values) condition ];
```

```
//原始数组
var numbers = [0,1,2,3,4,5,6,7,8,9,10];
//把所有元素复制到新数组
var duplicate = [i for each (i in numbers)];
//只把偶数复制到新数组
var evens = [i for each (i in numbers) if (i % 2 == 0)];
//把每个数乘以 2 后的结果放到新数组中
var doubled = [i*2 for each (i in numbers)];
//把每个奇数乘以 3 后的结果放到新数组中
var tripledOdds = [i*3 for each (i in numbers) if (i % 2 > 0)];
```

## 待续
发现很多属性和方法已经不可用了,暂时先看到这里










