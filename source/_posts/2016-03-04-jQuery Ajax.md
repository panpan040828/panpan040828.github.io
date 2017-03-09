---
layout: post
title: jQuery Ajax
date: 2016-03-04
categories: 前端笔记 
tags: [jQuery,Ajax]
description: 
---

### 一. Ajax方法

**jquery**：JavaScript代码包装成拿过来就能实现特定功能的代码库

**json**：简单说就是javascript中的对象和数组

**Ajax**：异步的JavaScript和XML，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示

##### 1. $(selector). load(url,[data],[callback]) 获取网页

**url**：服务器的地址

可选项**data**参数为请求时发送的数据
    
**callback**参数为数据请求成功后，执行的回调函数
    
eg: 
` .load("http://www.imooc.com/data/fruit_part.html"  ,function(){`
                        `$this.attr("disabled", "true");`
                   ` });`

##### 2.    jQuery.getJSON(url,[data],[callback]) 或者 $.getJSON(url,[data],[callback])  获取数组

使用<font color="red">getJSON()</font>方法可以通过<font color="red">Ajax异步请求</font>的方式，获取服务器中的数组，并对获取的数据进行解析

##### 3. jQuery.getScript(url,[callback])  或 $.getScript(url,[callback])

异步加载并执行js文件  获取JS文件内容

##### 4. $.get(url,[callback])     向服务器请求数据

 采用GET方式向服务器请求数据，并通过方法中回调函数的参数返回请求的数据
 
##### 5. $.post(url,[data],[callback])   向服务器发送数据

用于以POST方式向服务器发送数据，服务器接收到数据之后，进行处理，并将处理结果返回页面

eg:    ` $.post("http://www.imooc.com/data/check_f.php",{num:$("#txtNumber").val()},`
                    `function (data) {});`
                    
 <font color="red">注意：Ajax里面的post、get都是以用户为主体，所以get指的是向服务器请求数据，post是指向服务器发送数据</font>

##### 6.  $(selector).serialize()        将元素进行序列化

将表单中有name属性的元素值进行序列化，生成标准URL编码文本字符串???，直接可用于ajax请求

##### 7. jQuery.ajax([settings])   或$.ajax([settings])

获取服务器数据，同时向服务器发送请求
  
使用**$.ajax()**方法是最底层、功能最强大的请求服务器数据的方法，
它不仅可以获取服务器返回的数据，还能向服务器发送请求并传递数值

settings为一个**对象**：该对象包括：

url  表示服务器请求的路径，

 data为请求时传递的数据，
                                               
 dataType为服务器返回的数据类型
                                               
 success为请求成功的执行的回调函数
                                               
type为发送数据请求的方式，**默认为get**
                                               
eg:          	`$.ajax({`
                         `	url:"http://www.imooc.com/data/check.php",`
                         	`data: { num: $("#txtNumber").val() },`
                         `	dataType:"text",`
                         `	success: function (data) {`
                        ` 	$("ul").append( "<li> 你输入的<b>  "  + $("#txtNumber").val() + " </b>是<b> "  + data + " </b></li>");`
                       	 `}`
                   	 `});`

##### 8. jQuery.ajaxSetup([options])     或$.ajaxSetup([options])         

设置Ajax的全局性选项值
   
设置Ajax请求的一些全局性选项值，设置完成后，后面的Ajax请求将不需要再添加这些选项值，options为一个对象

##### 9.  $(selector).ajaxStart(function())  ajax请求执行前执行

$(selector).ajaxStop(function())    ajax请求执行后执行

当发送Ajax请求前，执行ajaxStart()方法绑定的函数；请求成功后，执行ajaxStop ()方法绑定的函数











    






	













