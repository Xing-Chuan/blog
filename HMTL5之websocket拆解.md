---
title: HTML5之WebSocket拆解
tags:
  - WebSocket
  - HTML5
  - 通讯协议
categories: HTML
date: 2017-08-09 11:39:36
---

客户端要即时获取服务端的消息, 以前的做法是使用 Ajax长轮询 或 commet 持久连接来实现, 但这样很耗费服务端带宽资源, 使用长轮询需要服务器有优秀的响应速度, 持久连接则需要服务端有良好的高并发能力.

基于这种现状, 才有了 WebSocket 通讯协议的诞生.

## 为什么需要 WebSocket ?

目前的网络数据的传输是遵循 HTTP 1.1 传输的, 目前 HTTP协议 的缺点就是请求只能从客户端发起, 服务端无法对客户端推送消息, 所以才有长轮询和 commet 这些解决方案. 为了解决这种现状, 才有了 WebSocket 的诞生.

## WebSocket 有什么特点?

(1) 建立在 TCP/IP 协议之上, 兼容性好, 客户端和服务端比较容易实现

(2) 没有浏览器同源限制, 服务端不用设置 CORS

(3) 客户端和服务端互相可以实时推送

(4) 协议标识符使用 `ws` , 如果加密则使用 `wss`

(5) 一次连接, 持久有效, 有状态连接

### WebSocket 和 HTTP 的区别

WebSocket 和 HTTP 的区别可以借用下面两张图表示( 侵删 ):

![image.png](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051502.png)

![image.png](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)


## WebSocket API

因服务端语言各有实现方式, 这里阐述下客户端的 API:

```js
// 创建 Socket 实例
let ws = new WebSocket("wss://echo.websocket.org");

// 连接成功
ws.onopen = function(evt) {
  console.log("Connection open ...");
  // 初始化消息
  ws.send("Hello WebSockets!");
};

// 发送消息
ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  // 关闭 Socket
  ws.close();
};

// 关闭连接或停止正在进行的链接请求
ws.onclose = function(evt) {
  console.log("Connection closed.");
};
```

详细属性可点击[这里](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)查看.

## WebSocket 应用

虽然现代浏览器都已经支持 WebSocket , 也有 IE 这样不出乎意料的例外, 所幸目前有一款集成和兼容性都比较好的封装, `socket.io` .

Socket.IO 使用检测功能来判断是否建立 WebSocket 连接，或者是 AJAX long-polling 连接，或 Flash 等。可快速创建实时的应用程序.

### 基于 Nodejs 实现一个简单的聊天室应用

```js
// nodejs 服务端代码
let app = require('express')();
let http = require('http').Server(app);
let io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  socket
    .on('chat message', function(msg){
      io.emit('chat message', msg);
    });
});

http.listen(3000, function(){
  console.log('listening start, port: 3000');
});
```

```html
<!-- HTML 客户端代码 -->
<ul id="messages"></ul>
<form action="">
  <input id="m" autocomplete="off" />
  <button type="submit">Send</button>
</form>
<script src="https://code.jquery.com/jquery-1.11.1.js"></script>
<script src="/socket.io/socket.io.js"></script>
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
    socket.on('chat message', function(msg){
      $('#messages').append($('<li>').text(msg));
    });
  });
</script>
```