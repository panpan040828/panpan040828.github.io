---
layout: post
title: web worker
date: 2016-09-12
categories: javaScript
tags: [javaScript]
description: 
---

### 一. 浏览器的js引擎是单线程地处理任务队列

浏览器多线程
js是单线程

ajax请求是异步的，是由**浏览器新开一个线程**请求，事件回调的时候放入事件队列等候处理。

### 二. 如何实现js的多线程

web worker是html5中提出的一个javascript多线程解决方案。

**在主线程中：**

```
//创建一个Worker对象，并将新线程代码的url作为参数传入
var worker = new Worker('worker.js');

//向worker发送数据
worker.postMessage({type: 'command', message: 'start!'});

//监听worker向主线程发送数据，数据保存在onmessage事件的event.data中
worker.onmessage = function(event) {
	console.log(event.data); //输出worker发送来的数据
}
```

**在新开的线程中(worker.js文件中)：**

```
//获取从主线程中传来的数据
this.onmessage = function(event) {
	var data = event.data;

	//对数据进行处理
	...

	//将处理之后的数据发回给主线程
	this.postMessage(data);
}
```

**主线程中的API：**

```
var worker = new Worker('...')
worker.postMessage()
worker.onmessage
worker.onerror
worker.terminate()
```

**新开的线程中的API：**

在新开的线程中，self和this都是指的worker对象，不能访问页面中的DOM

```
this.postMessage()
this.onmessage
this.close()
```

注意：在本地文件中不能运行web worker

XMLHttpRequest的异步执行模式，相当于是一个拥有专用API的web worker。