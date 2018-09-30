---
title: Socket.io基本知识
date: 2018-03-17 16:52:11
tags: NodeJs
---
## Scoket是什么？
基于时间的实时双向通信库
基于webscoket协议
前后端通过事件进行双向通信

## Scoket.io与Ajax区别
### 基于不同的网络协议
Ajax基于http协议，单向，实时获取数据只能轮询
- HTTP协议，它规定了在网络中发布、传输和接收html页面的方法。大家都遵循这个协议，就能实现信息的传输。
- 客户端和服务器端没有任何联系——建立连接，客户端发送请求——沿着建立好的连接，服务器端返回响应信息——断开连接。
- HTTP请求信息的格式。 
请求信息分为三部分：请求行、请求头信息和请求主体信息。请求头信息和请求主体信息之间用一个空行分割，不管是否有请求主体信息，这个空行都必须存在。
请求行又分成三部分：请求方法，请求资源的路径，所用协议的版本(目前一般是http1.1，0.9和1.0已经基本不用了)。

> 请求方法又有以下几种：GET/POST/PUT/DELETE/TRACE/OPTIONS等。
get方式通过地址栏传递参数，post方式是通过请求头信息传递信息的。但是这两种方式传递数据的格式都是相同的。key=value&key=value

响应行：协议版本，状态码，状态描述信息。

socket.io基于websocket双向通信协议，后端可以主动推送数据

#### websocket是什么
Websocket是html5提出的一个协议规范，参考rfc6455。
websocket约定了一个通信的规范，通过一个握手的机制，客户端（浏览器）和服务器（webserver）之间能建立一个类似tcp的连接，从而方便c－s之间的通信。在websocket出现之前，web交互一般是基于http协议的短连接或者长连接。

WebSocket是为解决客户端与服务端实时通信而产生的技术。websocket协议本质上是一个基于tcp的协议，是先通过HTTP/HTTPS协议发起一条特殊的http请求进行握手后创建一个用于交换数据的TCP连接，此后服务端与客户端通过此TCP连接进行实时通信。
注意：此时不再需要原HTTP协议的参与了。

#### websocket的优点
以前web server实现推送技术或者即时通讯，用的都是轮询（polling），在特点的时间间隔（比如1秒钟）由浏览器自动发出请求，将服务器的消息主动的拉回来，在这种情况下，我们需要不断的向服务器发送请求，然而HTTP request 的header是非常长的，里面包含的数据可能只是一个很小的值，这样会占用很多的带宽和服务器资源。

而最比较新的技术去做轮询的效果是Comet – 用了AJAX。但这种技术虽然可达到全双工通信，但依然需要发出请求(reuqest)。

WebSocket API最伟大之处在于服务器和客户端可以在给定的时间范围内的任意时刻，相互推送信息。 浏览器和服务器只需要要做一个握手的动作，在建立连接之后，服务器可以主动传送数据给客户端，客户端也可以随时向服务器发送数据。 此外，服务器与客户端之间交换的标头信息很小。

WebSocket并不限于以Ajax(或XHR)方式通信，因为Ajax技术需要客户端发起请求，而WebSocket服务器和客户端可以彼此相互推送信息；

![Socket通信模块](http://upload-images.jianshu.io/upload_images/2088736-ed5f3665717f4db4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Socket.io使用
### Socket.io后端API
`npm install socket.io --save`
配合`express`
`io = require('socket.io')(http)`
`io.on`监听事件
`io.emit`触发事件
```
const express = require('express');
const port = 9093;
// 新建app
const app = express();
// work with express 
const server = require('http').Server(app);
const io = require('socket.io')(server);
// io全局的请求
io.on('connection',function(socket){
	console.log('user login')
	// socket当前的请求
	socket.on('sendmsg',function(data){
		console.log(data);
		// 转为全局接收状态
		io.emit('recvmsg',data);
	})
})

server.listen(port,function(){
	console.log('Node app start at port '+port+"!");
})
```
### Socket.io前端API
`npm install socket.io-client --save`
配合`express`
`import io from 'socket.io-client'`
`io.on`监听事件
`io.emit`触发事件
```
import io from 'socket.io-client';
const socket = io('ws://localhost:9093');

socket.on('recvmsg',(data)=>{
	console.log(data);
})
```