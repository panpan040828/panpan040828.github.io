---
layout: post
title: Baidu IFE 第一阶段任务1~6
date: 2016-03-15
categories: Demo
tags: [BaiduIFE]
description: 参加百度前端公开课的第一阶段任务
---

### 一. 百度前端技术学院简介

百度前端技术学院诞生于2014年，由百度最大规模的前端技术组织 EFE 团队发起成立。

目前学院是由百度校园品牌部、百度多模交互搜索部以及百度校园招聘组联合组织的，面向大学生人群，免费的一个技术学习、交流与分享平台。

### 二.  第一阶段任务

![第一阶段任务](/uploads/post/demo/baidu-ife-task1.jpg)

### 三.  我的任务

我的任务列表[http://panpanfish.com/baiduIFE/](http://panpanfish.com/baiduIFE/)

在写这个任务列表时遇到的问题：

**子元素浮动之后如何撑开父元素**

由于子元素添加float属性之后就脱离了文档流，给父元素设定一个overflow:hidden;属性，便可使子元素float之后撑开父元素。

**float的元素如何居中显示**

如下图所示：

![浮动元素居中显示](/uploads/post/demo/float-center.jpg)

#### 1. 任务一

#### (1). 任务一需要完成的示意图

![](http://7xrp04.com1.z0.glb.clouddn.com/task_1_1_1.jpg)


**任务要求：**

- 只需要完成HTML代码编写，不需要写CSS
- 示例图仅为参考，不需要完全实现一致，其中的图片、文案均可自行设定
- 尽可能多地尝试更多的HTML标签

#### (2). 任务一demo

[http://panpanfish.com/baiduIFE/task1-1.html](http://panpanfish.com/baiduIFE/task1-1.html)

#### (3). 任务一代码

[https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-1.html](https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-1.html)

#### (4). 任务一笔记

1.  html5里面添加了很多语义化更好的内容标签：

	article、footer、header、nav、section

	使用这些标签会使代码结构更加清晰
 
1. div、section、article三者的相同点和不同

	**相同点：**都是块元素，可以对某一块内容添加样式

	**不同点：**div没有语义，仅仅用作样式化或者脚本化；section适用于一段主题内容；article适用于一段完整的独立存在的内容（判断方式：此段内容可不可以脱离上下文）

#### 2. 任务二

#### (1). 任务二需要完成的示意图

![](http://7xrp04.com1.z0.glb.clouddn.com/task_1_2_1.jpg)

**任务要求：**

- 只需要完成HTML，CSS代码编写，不需要写JavaScript
- 示例图仅为参考，不需要完全实现一致，其中的图片、文案均可自行设定
- 尽可能多地尝试不同的、更多的样式设定来实践各种CSS属性
- HTML 及 CSS 代码结构清晰、规范

#### (2). 任务二demo

[http://panpanfish.com/baiduIFE/task1-2.html](http://panpanfish.com/baiduIFE/task1-2.html)

#### (3). 任务二代码

[https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-2.html](https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-2.html)

#### (4). 任务二笔记

主要是记录一些之前没用到的css样式

1. 让一段文字的首行缩进2个字符

    text-indent:2em;

2. 让一行文字垂直居中于一个div

    line-height = div的height

3. 使用到的css3新增加的特性

    实现阴影 box-shadow
    
    实现圆角 border-radius


4. verticle-align 设置元素垂直对齐方式

    verticle-align: 百分比 （这个百分比是相对于line-height属性值）

	若为正值，则升高；若为负值，则降低；0%等同于baseline。

#### 3. 任务三

#### (1). 任务三需要完成的示意图

![](http://7xrp04.com1.z0.glb.clouddn.com/task_1_3_1.png)

**任务要求：**

- 左右两栏宽度固定，中间一栏根据父元素宽度填充满，最外面的框应理解为浏览器。
- 调节浏览器宽度，固定宽度和自适应宽度的效果始终符合预期。
- 改变中间一栏的内容长度，以确保在中间一栏较高和右边一栏较高时，父元素的高度始终为子元素中最高的高度。背景色为 #eee 区域的高度取决于三个子元素中最高的高度。
- 其他效果图中给出的标识均被正确地实现。

#### (2). 任务三demo

[http://panpanfish.com/baiduIFE/task1-3.html](http://panpanfish.com/baiduIFE/task1-3.html)

#### (3). 任务三代码

[https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-3.html](https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-3.html)

#### (4). 任务三笔记

1. 上传的这个demo是利用左右float，中间设定margin来实现的
	
左右固定宽度，中间自适应

    <!DOCTYPE HTML>
    <html>
    	<head>
    	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
    		<title>左右固定宽度，中间自适应</title>
    		<style type="text/css">
    			.leftside-bar{float:left;width:200px;height:100px;background: red;}
    			.rightside-bar{float:right;width:200px;height:100px;background: black;}
    			.content{margin-left:210px;margin-right:210px;height:100px;background: grey;}
    		</style>
    		<script type="text/javascript">
    			
    		</script>
    	</head>
    	<body>
    		<div class="leftside-bar"></div>
    		<div class="rightside-bar"></div>
    		<div class="content"></div>
    		
    	</body>
    </html>

2. 脱离文档流

	**什么叫元素脱离文档流？**

	元素从普通布局排版中拿走，其他盒子在定位的时候，会把脱离文档流的元素当做不存在来进行定位。

	**什么情况下元素会脱离文档流？**

	元素设置了float属性 或者 元素的position定义为absolute

    **float脱离文档流 和 absolute脱离文档流的不同之处？**

	float脱离文档流：其他盒子会无视这个元素，但文字会为其让出位置，环绕周围

	absolute脱离文档流：其他盒子和文字均会无视这个元素

3. box-sizing的几种取值

	box-sizing可以理解为选择定义盒子尺寸的模式

	box-sizing的取值有 content-box、border-box、padding-box

	适用于一切有width、height属性的元素

	content-box：width和height均指的是 **内容** 的宽度和高度（默认值）

	border-box：width和height指的是 **内容+padding+border**的宽度和高度

	padding-box：width和height值的是 **内容+padding**的宽度和高度

#### 4. 任务四

#### (1). 任务四需要完成的示意图

![](http://7xrp04.com1.z0.glb.clouddn.com/task_1_4_1.png)

**任务要求：**

- 左右两栏宽度固定，中间一栏根据父元素宽度填充满，最外面的框应理解为浏览器。
- 调节浏览器宽度，固定宽度和自适应宽度的效果始终符合预期。
- 改变中间一栏的内容长度，以确保在中间一栏较高和右边一栏较高时，父元素的高度始终为子元素中最高的高度。背景色为 #eee 区域的高度取决于三个子元素中最高的高度。
- 其他效果图中给出的标识均被正确地实现。

#### (2). 任务四demo

[http://panpanfish.com/baiduIFE/task1-4.html](http://panpanfish.com/baiduIFE/task1-4.html)

#### (3). 任务四代码

[https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-4.html](https://github.com/panpan040828/baiduIFE/blob/gh-pages/task1-4.html)

#### (4). 任务四笔记

这个任务中需要实现的效果有四个：灰色块水平居中、灰色块垂直居中、黄色四分之一圆的实现、黄色四分之一圆的位置固定

1. 灰色块水平居中

	设置灰色块的 `margin-left:auto; margin-right:auto;`

2. 灰色块的垂直居中

	关于垂直居中的问题，网上有很多人提出了很多种实现方式

	在这里我只用了一种我感觉最好理解，并且适用于所有浏览器的方式

	    <div class="floater"></div>
    	<div class="content">
       		Content here
    	</div>
    
    	html,body{margin:0px;height:100%} /*高度自适应的关键*/
    	
    	.floater {float:left;
      			 height:50%;/*指的是这个悬浮在左边的div的高度为浏览器高度的一半*/
      			margin-bottom:-120px;/*将此div向上移动内容div高度的一半*/
      	}
    	.content {clear:both; height:240px; position:relative;}

3. 关于height:50%

	需要注意一个对象的高度是否可以使用百分比显示，取决于对象的父级元素，在浏览器默认状态下，是没有给body、html一个高度属性的。

	因此我们需要设置 html,body{height:100%}

	**很奇怪的一点：若在html文件的开头没有声明<!DOCTYPE HTML>，不设置也不会有问题，这个问题还没搞懂。**

4. border-radius属性

	如何画四分之一圆？

    div的宽和高相等，并且等于border-radius的某一个角的弧度

#### 5. 任务五

见博客《常见的两栏式布局》

#### 6. 任务六

见博客《模拟报纸排版》