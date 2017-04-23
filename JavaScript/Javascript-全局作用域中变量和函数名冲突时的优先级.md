---
title: 全局作用域中变量和函数名冲突时的优先级
tags: [作用域,优先级]
date: 2016-3-3 19:21:43
categories: JavaScript
---


# 全局作用域中变量和函数名冲突
- 全局作用域预解析时,如果变量与函数名相同,以函数为准
- 变量有赋值时,以变量为准

## 全局作用域变量名和函数名冲突
```js
    // 全局作用域变量名和函数名冲突
    // 1
    var a1=123;
    console.log(typeof a1); // number
    function a1() {}
    // 2
    console.log(typeof a2); // function
    var a2=123;
    function a2() {}
    // 3
    var a3=123;
    function a3() {}
    console.log(typeof a3); // number
    // 4
    var a4;
    function a4() {}
    console.log(typeof a4); // function
    a4=123;
```
## 预解析时,变量名、函数名、和传参冲突
```js
// 预解析时,变量名、函数名、和传参冲突
    // 1.预解析,var a 先提升,实参给a赋值,后被222覆盖,输出222
    function foo1(a){
        var a = 222;
        console.log(a); //222
    }
    foo1(111);
    // 2.预解析,var a 先提升,实参先给a赋值,输出111,后被222覆盖
    function foo2(a){
        console.log(a); //111
        var a = 222;
    }
    foo2(111);
    // 3.变量已经被赋值,输出222
    function foo3(a){
        var a = 222;
        console.log(a); //222
        function a() {}
    }
    foo3(111);
    // 3.变量没有被赋值时,输出a的函数体
    function foo4(a){
        var a;
        console.log(a); //a的函数体
        a = 222;
        function a() {console.log("qwe")}
    }
    foo4(111);
```
