---
layout: post
title: Web Sockets协议
date: 2016-06-01
categories: HTML
tags: [HTML5]
description: 
---

### 一. 什么是Web Sockets?

Web Sockets是一个实现双向实时通信的**协议**

Web Sockets的目的是：

> 在一个单独持久的连接上提供全双工、双向通信

### 二. web sockets 使用

#### 1. 创建一个WebSocket实例

```js
var socket = new WebSocket("ws://www.example.com/server.php");
```

注意：传入的参数是一个绝对的URL，websocket协议不受同源策略的影响。

#### 2. 向服务器发送数据 send()方法

```js
socket.send("Hello world");
```
注意：websocket只能通过send发送纯文本数据，对于复杂的数据结构，需要在发送之前进行序列化。

例如：发送JSON数据

```js
var message = {
	time: new Date(),
	text: "hello world",
	clientId: "asdfp8734rew"
}

socket.send(JSON.stringify(message));
```

插叙一点关于JSON的知识：

JSON是一种轻量级的文本数据交换格式，在语法上与js对象的代码相同。

**JSON.parse()**和**JSON.stringify()**比较

- JSON.parse()：从一个字符串中解析出json对象
- JSON.stringify()：将对象变成字符串

#### 3. 从服务器接收数据

当服务器向客户端发送消息时，websocket对象会触发**message事件**，返回的数据保存在event.data属性中。

```js
socket.onmessage = function(event){
	var data = event.data;
	//然后处理获得的数据data
}
```
event.data中保存的也是字符串

### 三. websocket对象

#### 1. websocket对象的属性

WebSocket属性**readState**取值：

- WebSocket.OPENING(0):正在建立连接
- WebSocket.OPEN(1):已经建立连接
- WebSocket.CLOSING(2):正在关闭连接
- WebSocket.CLOSE(3):已经关闭连接

#### 2. websocket对象的方法

- send()
- close()

#### 3. websocket对象的事件

- message：服务器向客户端发送消息时会触发
- open：客户端与服务器成功建立连接时触发
- error：在发生错误时触发，连接不能持续
- close：在连接关闭时触发

**close事件的event有额外的信息：**

- wasClean:布尔值，是否明确地关闭
- code：服务器返回的数值状态码
- reason：字符串，包含服务器发回的消息




