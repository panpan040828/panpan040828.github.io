---
layout: post
title: 编写高质量代码————javaScript
date: 2016-06-21
categories: 前端笔记
tags: [读书笔记,javaScript]
description: 
---

### 一. 高质量的javaScript

#### 1. 团队合作——如何避免JS冲突

    工程师甲定义了一个全局变量a
    
    var a, b = "hello";
    
    工程师乙也定义了一个全局变量a
    
    var a = "la";

这种情况下，就会团队合作就会产生冲突。

<font color="red">**解决办法：**</font>

用匿名函数将脚本包起来，可以将变量的作用域控制在匿名函数之内,可有效控制全局变量。

    工程师甲的代码：
    (function(){
    	var a, b = "hello";
    });
    
    工程师乙的代码：
    (function(){
    	var a = "la";
    });

### 2. 团队合作——两个功能模块之间如何进行通信

使用1中的匿名函数包裹法，让每个功能呢的变量都限定在自己的作用域内，那么如果工程师乙想要使用工程师甲代码里的变量，应该怎么办呢？

<font color="red">**解决办法：**</font>

使用全局变量进行通信，声明一个全局变量var GLOBAL = {};并使用命名空间。

	<script>       
        var GLOBAL = {};//声明一个全局的对象

		GLOBAL.namespace = function(str) {
			var arr = str.split("."), o = GLOBAL;
			for(var i = (arr[0] == "GLOBAL") ? 1 : 0; i < arr.length; i++) {
				o[arr[i]] = o[arr[i]] || {};
				o = o[arr[i]];
			}
		}
    </script> 
    
	//工程师甲的代码
    <script> 
      (function() {
        var a = "hello";
        GLOBAL.A = {};//命名空间为A
        GLOBAL.A.str = a;
        console.log(GLOBAL);//{A:{str:"hello"}}
      })();
    </script> 
    
	//工程师乙的代码
    <script>  
      (function() {
        var b = GLOBAL.A.str;//这样乙的功能模块里就使用了甲里面的变量
        console.log(GLOBAL);//{A:{str:"hello"}}
      })();
    </script> 

#### 3. 给所有的功能提供一个统一的入口

    js分为框架部分和应用部分。
    
    框架部分用来定义全局变量，命名空间，与具体的功能无关。
    
    应用部分全部放在一个`init()`函数里面，用`window.onload = init;`调用初始化函数。

**注意：**当页面的图片、媒体文件较多时，使用window.onload可能并不好，因为我们一般只需要等DOM元素加载完毕之后即可。

#### 4. CSS放在页头，js放在页尾

#### 5. 压缩js

**压缩js所做的功能：**去掉空格、去掉换行、将复杂的变量名变成简单的变量名。

#### 6. js分层

推荐的一种分层的方法：

- base层
- common层：提供可以复用的组件
- page层：与某一页面的具体功能相关

**(1)base层**

封装不同浏览器之间的差异，提供统一的接口（如前5个例子）；扩展js底层的接口。

分为三类：event相关，dom相关，language相关

**例子一：**

<font color="red">封装一个取得下一个DOM节点的函数`getNextNode`</font>

    由于js中的nextSibling属性获得的相邻的下一个节点并不一定是dom节点，可能是空的文本节点等。

    function getNextNode(node) {
	    var nextNode = node.nextSibling;
	    if(!nextNode) {
	      return null;
	    }
	    
	    while(true) {
	      if(nextNode.nodeType == 1) {
	    	break;
	      } else {
		    if(nextNode.nextSibling) {
		      nextNode = nextNode.nextSibling;
		    } else {
		      break;
		    }
	      }
	    
	      return nextNode;
	    }
    }

**例子二：**

<font color="red">IE和其他浏览器中的事件对象`event`，封装一个获取当前事件的派生对象的函数`getEventTarget`</font>

    在IE中，event是作为window对象的属性存在的；在其他浏览器中，event是事件的参数。
    
    在IE中，派生事件的对象是由`event.srcElement`获得；在其他浏览器中，派生事件的对象是由`event.target`获得。
    
    function getEventTarget(event) {
    	var eve = window.event || event;
    	return eve.srcElement || eve.target;
    }

**例子三：**

<font color="red">取消事件冒泡</font>

    在IE中，使用`event.cancelBubble = true`来取消事件冒泡。
    
    在其他浏览器中，使用`stopPropagation()`方法来取消事件冒泡。
    
    function stopPropagation(event) {
    	var eve = window.event || event;
    	if(eve.stopPropagation) {
    		eve.stopPropagation();
    	} else {
    		eve.cancelBubble = true;
    	}
    }

**例子四：**

<font color="red">取消事件的默认行为</font>

    **事件的默认行为：**例如`<a>`的点击事件的默认行为就是跳转链接
    
    在IE中，使用`event.returnValue = false`来取消事件的默认行为。
    
    在其他浏览器中，使用`preventDefault()`方法来取消事件的默认行为。
    
    function preventDefault() {
    	var eve = window.event || event;
    	if(eve.preventDefault){
    		eve.preventDefault();
    	} else {
    		eve.returnValue = false;
    	}
    }

**例子五：**

<font color="red">绑定事件和取消绑定事件</font>

    IE(IE8及以下版本)中使用obj.attachEvent("on...",fn)绑定事件；使用obj.detachEvent("on...",fn)来解绑事件。
    
    其他浏览器中使用obj.addEventListener("type",fn,false)来绑定事件；使用obj.removeEventListener("type",fn,false)来解绑事件
    
    var EventUtil = {
    
    	addHandler: function(element,type,fn) {
    		if(element.attachEvent) {
    			element.attachEvent("on" + type,fn);
    		} else {
    			element.addEventListener(type,fn,false);//如果允许事件冒泡，第三个参数则为false
    		}
    	},
    
    	removeHandler: function(element,type,fn) {
    		if(element.detachEvent) {
    			element.detachEvent("on" + type,fn);
    		} else {
    			element.removeEventListener(type,fn,false);
    		}
    	}
    
    }

-----------------------------------------------------

下面是扩展js的底层接口的例子。

**例子六：**

<font color="red">判断类型的方法</font>

    //基本类型
    function isNumber(s) {
    	return !isNaN(s);
    }
    
    function isString(s) {
    	return typeof s === "string";
    }
    
    function isBoolean(s) {
    	return typeof s === "boolean";
    }
    
    function isNull(s) {
    	return s === null;
    }
    
    function isUndefined(s) {
    	return typeof s === "undefined";
    }
    
    //判断是否为空
    function isEmpty(s) {
    	return /^\s*$/.test(s);//正则表达式.test(字符串)
    }
    
    //引用类型判断
    function isArray(s) {
    	return s instanceof Array;
    }
    
    function isFunction(s) {
    	return typeof s === "function";
    }

**例子七：**

<font color="red">用$代替document.getElementById("")</font>

    由于document.getElementById("")太长了，可以写一个函数，用一个简单的函数名来代替它。
    
    function $(node) {
    	node = typeof node === "string" ? document.getElementById(node) : node;//传入的参数是id值或者节点本身
    	return node;
    }

**例子八：**

<font color="red">使用className获取节点的getElementsByClassName()</font>

**用id名获取节点：**一个页面的同一个id只能出现一次，因此不能通过id来获取一组相似功能的节点。

**用tag名获取节点：**程序和HTML耦合得太紧。
	
	//通过className来获取一组dom节点
	//输入参数：str为class的名字，必须；
	//root为该dom的父元素，可选
	//tag为标签名，可选
    function getElementByClassName(str,root,tag) {
    	if(root) {
    		root = typeof root === "string" ? document.getElementById(root):root;
    	} else {
    		root = document.body;
    	}
    
    	tag = tag || "*";
    	var els = root.getElementByTagName(tag), arr = [];
    	for(var i = 0; i < els.length; i++) {
			var  elsClassName = els[i].className.split(" ");
			for(var j = 0; j < elsClassName.length; j++) {
				if(elsClassName[j] == str) {
	    			arr.push(els[i]);
	    			break;
    			}
			}   		
    	}
    	return arr;
    }

**例子九：**

<font color="red">增加className和删除某一个className</font>

使用dom.className = ""；来删除className，但是这种方法有一个缺陷，就是会将dom元素的所有className都删除。所以需要封装可以删除或者添加某一个特定的className的函数。

    //添加class的函数，输入参数node为该节点，str为class的名称
    function addClass(node,str) {
    	var reg = new RegExp("(^|\\s+)" + str);
    	if(!reg.test(node.className)) {
    		node.className = node.className + " " + str;
    	}
    }
    
    //删除class的函数，输入参数node为该节点，str为class的名称
    function removeClass(node,str) {
    	var reg = new RegExp("(^|\\s+)" + str);
    	node.className = node.className.replace(reg,"");
    }

**(2) common层**

common层一来base层提供的接口，为page组件调用。

**common层常见的组件有：**

- 异步通信的Ajax
- 用于拖拽的Drag
- 拖拉元素的Resize
- 动画的Animation
- 标签切换的Tab
- 树形目录的Tree
- 模拟弹出窗口Msg
- 用于拾色器的ColorPicker
- 用于日历的Calendar
- 用于富文本编辑器的RichTextEditor
- ............

这些很多我之前都做过，但是并没有封装成一个组件的形式，有时间应该好好整理一下。

    一个优秀的组件需要满足以下几个条件：
    
    - 跨浏览器兼容
    - 组件易用
    - 组件可重用
    - 组件可扩展
    - 代码组织有序，高内聚低耦合

common层没有base层那么通用，不是每一个页面都会使用到common层里面的所有组件，因此应该将common层的按组件的形式保存在不同的**.js文件**里，然后**按需加载**。

例如： common_tab.js、common_calendar.js

**(3) page层**

page层与具体的页面有关。

<font color="red">很多js框架可以为我们提供强大的base层和common层。</font>

#### 7. 编程小技巧

- 弹性
- 可复用性
- 避免产生副作用
- 通过传参实现定制
- 控制this关键字的指向
- 预留回调接口
- 编程中不要将相同的代码编写多次
- 用hash对象传参

可复用性：如果程序需要被同一个页面的多处复用，就不能用id来获取dom节点。

通过传参实现定制：将容易变化的因素通过参数传进来

利用hash对象传参的优点：参数的位置和顺序不重要了。