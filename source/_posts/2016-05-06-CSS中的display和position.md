---
layout: post
title: css中的display和position
date: 2016-05-06
categories: CSS
tags: [css]
description: 
---
### 一. display的取值

- none	
- block	
- inline	
- inline-block

**块级元素：**`<div>`、`<p>`、`<form>`、`<ul>`、`<ol>`、`<li>`

**行内元素：**`<span>`、`<strong>`、`<em>`

块级元素和行内元素的区别：

1. 块级元素会单独占一行；行内元素不会单独占一行
2. 块级元素可以设置width、height属性；行内元素不能设置width、height属性
3. 块级元素可以设置margin、padding属性；行内元素水平方向的margin、padding有效果，垂直方向没有

**display:none和 visibility:hidden的区别
**

- display:none 元素在视图中不占据空间
- visibility:hidden 元素在视图中保留占位

注意：display:none的元素也会被加载


### 二. position的取值

- inherit：继承其父元素的定位
- static：默认值，没有定位
- absolute：绝对定位
- relative：相对定位
- fixed

**absolute：**若其父元素有absolute/relative/fixed定位，那么相对于父元素进行定位；若没有，则相对于body进行定位

**relative：**相对于本身的位置进行定位

**fixed：**相对于浏览器窗口进行定位

**脱离文档流：**absolute、fixed、float

设置absolute和float都会让元素的display变成**inline-block**

z-index属性：

- 若元素position:static，那么文档流后面的元素会覆盖掉前面的
- 若元素设置了定位，那么z-index越大，元素会在越上方

默认情况下，所有元素都是在z-index：0这一层的，这就是文档流。





