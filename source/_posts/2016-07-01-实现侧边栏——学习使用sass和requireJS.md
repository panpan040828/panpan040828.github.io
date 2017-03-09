---
layout: post
title: 实现工具栏——学习使用Sass和requireJS
date: 2016-07-03
categories: Demo
tags: [模块化,Sass]
description: 
---

之前看过一些sass和模块化编程的知识，现在想通过这个小功能来运用一下。

### 一. 工具栏

工具栏demo地址：

[http://panpanfish.com/myDemo/toolbar/toolbar2.html](http://panpanfish.com/myDemo/toolbar/toolbar2.html)

工具栏代码地址：

[https://github.com/panpan040828/myDemo/tree/gh-pages/toolbar](https://github.com/panpan040828/myDemo/tree/gh-pages/toolbar)


### 二. HTML和css部分

实现常见的工具栏有两种方式：

- 使用背景图片，然后再使用css sprite技术进行定位
- 使用图标字体

#### 1. 两种方法的优缺点

<font color="red">使用背景图片 + css sprite的方式</font>

**优点：**

- HTML结构简单
- 兼容性良好

**缺点：**
- 使用了大量的图片，对性能有一定的影响
- 有一个最大的缺点就是，当你需要修改一些样式时，需要对图片进行修改，这样就会非常麻烦

css sprite主要是利用`background-image`，`background-position`

<font color="red">使用图标字体的方式</font>

**优点：**

避免了图片的使用，节约了性能，并且修改方便

**缺点：**

HTML结构复杂了一些
不兼容IE6和IE7

综上所述，还是使用图标字体的方式更好。

#### 2. 获取免费的图标的方式

下列网站上提供了很多免费的图标

[https://icomoon.io/app/#/select](https://icomoon.io/app/#/select)

(1) 选中自己需要的图标，点击右下角的`Generate Font`

(2) 然后将生成的图标下载下来（点击右下角的`Download`）

(3) 下载好的文件夹解压，解压后有用的文件只有

- fonts文件夹
- style.css文件

将上述文件放在项目的`css文件夹`里，然后将`style.css`里的代码放在自己的`css文件`里，并在`html`页面的图标标签里引用相应的样式即可。

#### 3. 使用sass来写css

第一次使用sass来写css，感觉确实很方便。

这里使用了sass的变量，混合器，嵌套，导入其他scss文件这几个功能。

**写scss时需要注意：**

- 运算符两边需要空格
- 在导入其他的scss文件时，可以不写命名前的下划线和后缀

#### 4. 这里的很多动画效果是利用才css3里面的属性完成的

**(1) transition属性**

修改图标的top值来改变显示，同时使用transition来平滑过渡

**(2) transform属性**

使用transform：scale()来改变图片的大小，同时使用transition来平滑过渡

改变转换的基准点可以使用属性`transform-origin`

这里需要注意不同浏览器之间的兼容性，这里利用了sass里面的@mixin。

### 三. js部分

这个工具栏需要用js来实现的功能就只有，第四个图标的返回顶部的功能。

虽然这只是一个很小的插件，但是为了学习使用requireJS，这里还是用了requireJS。

#### 1. requireJS的使用

(1) 首先需要在requireJS的官网上下载require.js放入js文件夹中

requireJS中文官网：[http://www.requirejs.cn/](http://www.requirejs.cn/)

(2) 然后再html里面引入reuqire.js，并指明入口文件

    <script src="js/require.js" data-main="js/toolbar.js"></script>

(3) toolbar.js里面的内容

```js
	requirejs(["base","common_backTop"], function(GLOBAL, backTop) {
		var topDiv = GLOBAL.Dom.$("backTop");
		console.log(topDiv);
		var top = new backTop.BackTop(topDiv, {
			mode: "move",
			des: 0,
			moveTime: 100
		});
	});
```

表明引入了两个模块，一个是js的基础模块`base.js`；另外一个是实现回到顶部功能的`common_backTop.js`。

(4) common_backTop.js模块的定义

```js
define(["base"], function(GLOBAL){
	var BackTop = function(ele, config) {
		this.ele = ele;
		this.config = config;
		var that = this;
		if(this.config.mode == "move") {
			GLOBAL.Eve.addEvent(this.ele, "click", function() {
				that.move();
			});
		} else {
			GLOBAL.Eve.addEvent(this.ele, "click", function() {
				that.go();
			});
		}		
		this.checkPos();
	}

	BackTop.prototype = {
		constructor: BackTop,

		move: function() {	
			var that = this.config;	
			function scroll(){
				var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
				if(scrollTop > that.des) {
					scrollTop -= 100;
					if(document.documentElement.scrollTop) {
						document.documentElement.scrollTop = scrollTop;
					} else {
						document.body.scrollTop = scrollTop;			
					}
					setTimeout(scroll, that.moveTime);
				} else if(scrollTop < that.des) {
					scrollTop += 100;
					if(document.documentElement.scrollTop) {
						document.documentElement.scrollTop = scrollTop;
					} else {
						document.body.scrollTop = scrollTop;			
					}
					setTimeout(scroll, that.moveTime);
				} else {
					return;
				}				
			}
			scroll();
		},

		go: function() {
			if(document.documentElement.scrollTop) {
				document.documentElement.scrollTop = this.config.des;
			} else {
				console.log(this.config);
				document.body.scrollTop = this.config.des;			
			}
		},

		checkPos: function() {
			var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
			if(scrollTop < document.body.clientHeight) {
				this.ele.style.display = "block";
			} else {
				this.ele.style.display = "none";
			}
		},
	}

	return {
		BackTop: BackTop
	}
});
```