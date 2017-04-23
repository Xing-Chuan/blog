---
title: 深复制与浅复制的区别
tags: [深复制]
date: 2016-03-02 10:32:12
categories: JavaScript
---

## 浅复制
浅拷贝是指复制对象的时候，指对第一层键值对进行独立的复制。一个简单的实现如下：
```js
// 浅复制实现
function shadowCopy(target, source){ 
    if( !source || typeof source !== 'object'){
        return;
    }
    if( !target || typeof target !== 'object'){
        return;
    }  
    // 这边最好区别一下对象和数组的复制
    for(var key in source){
        if(source.hasOwnProperty(key)){
            target[key] = source[key];
        }
    }
}
```
我们分别对对象和数组进行测试，发现成功地复制了。
```js
//测试例子
var arr = [1,2,3];
var arr2 = [];
shadowCopy(arr2, arr);
console.log(arr2);
//[1,2,3]

var today = {
    weather: 'Sunny',
    date: {
        week: 'Wed'
    } 
}

var tomorrow = {};
shadowCopy(tomorrow, today);
console.log(tomorrow);
// Object {weather: "Sunny", date: Object}
```
对于date这个属性，tomorrow变量复制的是它在堆中的地址，这也导致了today和tomorrow的date其实是指向堆中的同一个对象,只是对地址的引用。

```js
tomorrow.weather = 'Cloudy';
tomorrow.date.week = 'Thurs';

console.log(today.weather); // 'Sunny'
console.log(today.date.week); //'Thurs'
```

Array方法的slice，concat其实是一种浅复制
## 深复制
深复制的实现,只需要再浅复制的基础上使用递归,复制所有属性和值
```js
// 深复制实现
function deepCopy(target, source){ 
    if( !source || typeof source !== 'object'){
        return;
    }
    if( !target || typeof target !== 'object'){
        return;
    }  
    for(var key in source){
        if(source.hasOwnProperty(key)){
            if( source[key] && typeof source[key] == 'object'){
               target[key] = {};
               deepCopy(target[key], source[key]);
            } 
            else{
                target[key] = source[key];
            }    
        }    
    } 
}
```

### 深复制可以用JSON格式处理(ES5.API)
```js
var data=JSON.parse(JSON.stringify(obj))
// 缺点:对于一般的需求是可以满足的，但是它有缺点。
// 下例中，可以看到JSON复制会忽略掉值为undefined以及函数表达式。
var obj = {
    a: 1,
    b: 2,
    c: undefined,
    sum: function() { return a + b; }
};

var obj2 = JSON.parse(JSON.stringify(obj));
console.log(obj2);
//Object {a: 1, b: 2}
```
