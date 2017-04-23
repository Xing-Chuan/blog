---
title: setTimeout的秘密
tags:
  - JavaScript
  - 事件队列
date: 2016-06-03 14:21:36
categories: JavaScript
---

计时器setTimeout是我们经常会用到的，它用于在指定的毫秒数后调用函数或计算表达式。


语法：


> setTimeout(code, millisec, args);


注意：如果code为字符串，相当于执行eval()方法来执行code。


当然，这一篇文章并不仅仅告诉你怎么用setTimeout，而且理解其是如何执行的。


### 1、setTimeout原理


先来看一段代码：

~~~javascript
varstart = newDate(); 
 
varend = 0; 
 
setTimeout(function(){  
 
 console.log(newDate() - start); 
 
}, 500); 
 
while(newDate() - start <= 1000){}
~~~

<!--more-->

在上面的代码中，定义了一个setTimeout定时器，延时时间是500毫秒。


你是不是觉得打印结果是： 500


可事实却是出乎你的意料，打印结果是这样的（也许你打印出来会不一样，但肯定会大于1000毫秒）：





这是为毛呢？


究其原因，这是因为 JavaScript是单线程执行的。也就是说，在任何时间点，有且只有一个线程在运行JavaScript程序，无法同一时候运行多段代码。


再来看看浏览器下的JavaScript。


浏览器的内核是多线程的，它们在内核控制下相互配合以保持同步，一个浏览器至少实现三个常驻线程：JavaScript引擎线程，GUI渲染线程，浏览器事件触发线程。



	* JavaScript引擎是基于事件驱动单线程执行的，JavaScript引擎一直等待着任务队列中任务的到来，然后加以处理，浏览器无论什么时候都只有一个JavaScript线程在运行JavaScript程序。

	* GUI渲染线程负责渲染浏览器界面，当界面需要重绘（Repaint）或由于某种操作引发回流(Reflow)时，该线程就会执行。但需要注意，GUI渲染线程与JavaScript引擎是互斥的，当JavaScript引擎执行时GUI线程会被挂起，GUI更新会被保存在一个队列中等到JavaScript引擎空闲时立即被执行。

	* 事件触发线程，当一个事件被触发时，该线程会把事件添加到待处理队列的队尾，等待JavaScript引擎的处理。这些事件可来自JavaScript引擎当前执行的代码块如setTimeout、也可来自浏览器内核的其他线程如鼠标点击、Ajax异步请求等，但由于JavaScript的单线程关系，所有这些事件都得排队等待JavaScript引擎处理（当线程中没有执行任何同步代码的前提下才会执行异步代码）。




到这里，我们再来回顾一下最初的例子：

~~~javascript
varstart = newDate(); 
 
varend = 0; 
 
setTimeout(function(){  
 
 console.log(newDate() - start); 
 
}, 500); 
 
while(newDate() - start <= 1000){}
~~~

虽然setTimeout的延时时间是500毫秒，可是由于while循环的存在，只有当间隔时间大于1000毫秒时，才会跳出while循环，也就是说，在1000毫秒之前，while循环都在占据着JavaScript线程。也就是说，只有等待跳出while后，线程才会空闲下来，才会去执行之前定义的setTimeout。


最后 ，我们可以总结出，setTimeout只能保证在指定的时间后将任务(需要执行的函数)插入任务队列中等候，但是不保证这个任务在什么时候执行。一旦执行javascript的线程空闲出来，自行从队列中取出任务然后执行它。


因为javascript线程并没有因为什么耗时操作而阻塞，所以可以很快地取出排队队列中的任务然后执行它，也是这种队列机制，给我们制造一个异步执行的假象。


### 2、setTimeout的好搭档“0”


也许你见过下面这一段代码：

~~~javascript
setTimeout(function(){
 
 // statement
 
},0);
~~~

上面的代码表示立即执行。


本意是立刻执行调用函数，但事实上，上面的代码并不是立即执行的，这是因为setTimeout有一个最小执行时间，当指定的时间小于该时间时，浏览器会用最小允许的时间作为setTimeout的时间间隔，也就是说即使我们把setTimeout的延迟时间设置为0，被调用的程序也没有马上启动。


不同的浏览器实际情况不同，IE8和更早的IE的时间精确度是15.6ms。不过，随着HTML5的出现，在高级版本的浏览器（Chrome、ie9+等），定义的最小时间间隔是不得低于4毫秒，如果低于这个值，就会自动增加，并且在2010年及之后发布的浏览器中采取一致。


所以说，当我们写为 setTimeout(fn,0) 的时候，实际是实现插队操作，要求浏览器“尽可能快”的进行回调，但是实际能多快就完全取决于浏览器了。


那setTimeout(fn, 0)有什么用处呢？其实用处就在于我们可以改变任务的执行顺序！因为浏览器会在执行完当前任务队列中的任务，再执行setTimeout队列中积累的的任务。


通过设置任务在延迟到0s后执行，就能改变任务执行的先后顺序，延迟该任务发生，使之异步执行。


来看一个网上很流行的例子：

~~~javascript
document.querySelector('#one input').onkeydown = function(){  
 document.querySelector('#one span').innerHTML = this.value;  
};  
 
document.querySelector('#second input').onkeydown = function(){  
 setTimeout(function(){  
  document.querySelector('#second span').innerHTML = document.querySelector('#second input').value;  },0);
};
~~~

当你往两个表单输入内容时，你会发现未使用setTimeout函数的只会获取到输入前的内容，而使用setTimeout函数的则会获取到输入的内容。


这是为什么呢？


因为当按下按键的时候，JavaScript 引擎需要执行 keydown 的事件处理程序，然后更新文本框的 value 值，这两个任务也需要按顺序来，事件处理程序执行时，更新 value值（是在keypress后）的任务则进入队列等待，所以我们在 keydown 的事件处理程序里是无法得到更新后的value的，而利用 setTimeout(fn, 0)，我们把取 value 的操作放入队列，放在更新 value 值以后，这样便可获取出文本框的值。


未使用setTimeout函数，执行顺序是：onkeydown => onkeypress => onkeyup


使用setTimeout函数，执行顺序是：onkeydown => onkeypress => function => onkeyup


虽然我们可以使用keyup来替代keydown，不过有一些问题，那就是长按时，keyup并不会触发。


长按时，keydown、keypress、keyup的调用顺序：


keydown

keypress

keydown

keypress

...

keyup


也就是说keyup只会触发一次，所以你无法用keyup来实时获取值。


我们还可以用setImmediate()来替代setTimeout(fn,0)：


if(!window.setImmediate){  
 window.setImmediate = function(func,args){  
  returnwindow.setTimeout(func,0,args);  
 };  

 window.clearImmediate = window.clearTimeout; 
}


setImmediate()方法用来把一些需要长时间运行的操作放在一个回调函数里，在浏览器完成后面的其他语句后，就立刻执行这个回调函数，必选的第一个参数func，表示将要执行的回调函数，它并不需要时间参数。


注意：目前只有IE10支持此方法，当然，在Nodejs中也可以调用此方法。


### 3、setTimeout的一些秘密


#### 3.1 setTimeout中回调函数的this


由于setTimeout() 方法是浏览器 window 对象提供的，因此第一个参数函数中的this其实是指向window对象，这跟变量的作用域有关。


看个例子：

~~~javascript
vara = 1;  
varobj = {  
 a: 2,  
 test: function(){  
  setTimeout(function(){  
   console.log(this.a);  
  },0);  
 }  
};  
obj.test(); //  1
~~~

不过我们可以通过使用bind()方法来改变setTimeout回调函数里的this

~~~javascript
vara = 1;  
varobj = {  
 a: 2,  
 test: function(){  
  setTimeout(function(){  
   console.log(this.a);  
  }.bind(this),0);  
 }  
};  
obj.test(); //  2
~~~

#### 3.2 setTimeout不止两个参数


我们都知道，setTimeout的第一个参数是要执行的回调函数，第二个参数是延迟时间（如果省略，会由浏览器自动设置。在IE，FireFox中，第一次配可能给个很大的数字，100ms上下，往后会缩小到最小时间间隔，Safari，chrome，opera则多为10ms上下。）


其实，setTimeout可以传入第三个参数、第四个参数….，它们表示神马呢？其实是用来表示第一个参数（回调函数）传入的参数。

~~~javascript
setTimeout(function(a,b){  
 console.log(a); // 3
 console.log(b); // 4
},0,3,4);
~~~

