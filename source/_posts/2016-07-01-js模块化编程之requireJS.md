---
layout: post
title: js模块化编程之requireJS
date: 2016-07-01
categories: javaScript
tags: [javaScript,模块化]
description: 
---
###  一. 什么是requireJS？

js的模块化规范有3种：commonJs、AMD、CMD


### 二. requireJS的作用

- 实现js文件的异步加载
- 管理不同js文件之间的依赖，便于代码的编写和维护
- 防止命名冲突

### 三. requireJS的使用

**首先，在官网上下载requireJS后放在自己的项目的js文件夹里**

requireJS官网：[http://requirejs.org/](http://requirejs.org/)

**requireJS常用的方法：**

<font color="red">main.js主模块内使用require()</font>
   
    require([需要引入的模块], 回调函数(参数代表刚刚引入的模块){})；

    eg:

    require(["jquery"], function($) {
    
    });

<font color="red">使用require.config进行配置</font>

require.config()接受一个配置对象

    eg:
    
    require.config ({
    
    	//定义基目录
    	baseUrl: 'js/lib',
    	
        //定义别名
    	paths: {
    		jquery: "jquery-1.11.3.min"
    	},
    	
    	//加载不是使用AMD标准定义的模块
    	//引入underscore和backbone这两个库
    	//shim属性用来配置不兼容的模块
    	shim: {
    		'underscore': {
    			exports: '_'
    		},
    		
    		'backbone': {
    			deps: ['underscore', 'jquery'],
    			exports: 'Backbone'
    		}
    	}
    });


<font color="red">define：定义模块</font>
   
    define([需要引入的模块], function())

    eg:

    define("myModule", ["jquery"], function(参数代表刚刚引入的模块) {
    	return {
    		
    	}
    });
    通过return返回的函数，外面才能调用。
    
    其中模块的名字可以省略，因为AMD中的模块加载器可以将js文件名当做模块的名称。

使用data-main="js/main"来引入入口文件，入口文件指的是当requirejs加载完毕之后立即调用的js——>main.js。

<script type="text/javascript" src="js/require.js" data-main="js/main"></script>

### 四. requirejs的原理

requirejs可以看成一个模块加载器

1.我们在使用requireJS时，都会把所有的js交给requireJS来管理，也就是我们的页面上只引入一个require.js，把data-main指向我们的main.js。

 2.通过我们在main.js里面定义的requirejs方法或者define方法，requireJS会把这些依赖和回调方法都用一个数据结构保存起来。

 3.当页面加载时，requireJS会根据这些依赖预先把需要的js通过document.createElement的方法引入到dom中，这样，被引入dom中的script便会运行。

 4.由于我们依赖的js也是要按照requireJS的规范来写的，所以他们也会有define或者requirejs方法，同样类似第二步这样循环向上查找依赖，同样会把他们村起来。

 5.当我们的js里需要用到依赖所返回的结果时(通常是一个key value类型的object),requireJS便会把之前那个保存回调方法的数据结构里面的方法拿出来并且运行，然后把结果给需要依赖的方法。

 6.以上就是一个简单的流程。
