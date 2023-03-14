---
title: "socketio 使用方法"
catalog: true
date: 2022-01-03 21:16:28
subtitle: "学习经验."
header-img: 
tags:
- WebSocket
catagories:
- WebSocket
---

### 笔记描述
>  服务端是基于node的web框架express，在其基础上，使用socket.io模块来实现的。 
首先安装socket.io:

### 安装
>npm install --save-dev socket.io 

### 对应模块
```JS
var express = require('express');
var router = express.Router();
var app = require('express')();
var server = require('http').createServer(app);
var io = require('socket.io')(server);
io.on('connection', function(socket){
    console.log('a user connected');
   
  socket.on('sendMsg', (data) => {
    console.log(data)
    //   定义发送消息事件
    // io表示广播出去，发送给全部的连接
    io.emit('pushMsg', "服务端返回的消息：" + data)
  })
});
//开启端口监听socket
server.listen(3001);

router.get('/', function(req, res, next) {
    res.render('index');
});
 
module.exports = router;
```


### app使用
>npm i weapp.socket.io

或者引入脚本

```js
  <script 
        src="https://cdn.socket.io/4.4.0/socket.io.min.js" 
        integrity="sha384-1fOn6VtTq3PWwfsOrk45LnYcGosJwzMHv+Xh/Jx5303FVOXzEnw0EpLv30mtjmlj"
        crossorigin="anonymous">
    </script>


     const socket = io.connect('http://127.0.0.1:3000')
      function send(){
          var text = document.getElementById('text').value
        //   发送消息
          socket.emit('sendMsg',text)
      }

    //   接收服务端的消息
    socket.on('pushMsg',(data) => {
        console.log(data)
    })

```







