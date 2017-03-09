---
layout: post
title: HTML5中的canvas
date: 2016-04-25
categories: HTML
tags: [HTML5]
description: 
---
`<canvas>`是HYML5中新增的标签，利用canvas，可以在js内部完成图像的绘制，canvas可以理解成一张画布。

### 一. 使用canvas画图的准备工作

**HTML中**

	<canvas id="myCanvas"></canvas>

**JS中**

	var canvas1 = document.getElementById('myCanvas');//获取canvasDOM节点
        
    var ctx1 = canvas1.getContext('2d');//获取此canvas1的上下文，可以理解成获得画布

完成这两步之后，便可在画布上进行绘制

### 二. 使用canvas进行绘制

#### 1. 绘制图形  矩形、线型、三角形等

(1) 矩形

canvas只支持一种原生图形的绘制:矩形

与矩形有关的三个方法：

- fillRect(startX,startY,width,height)  绘制一个填充的矩形
- fillStroke(startX,startY,width,height) 绘制一个矩形的边框
- clearRect(startX,startY,width,height)  清除指定矩形区域

(2) 线型

通过设置路径来画各种线

	第一步：beginPath();//开始设置路径
	第二步：moveTo(x,y);//设置路径的起点（将笔触移动到某个坐标）
	第三步：lineTo(x,y);//设置线的终点，从起点到终点画一条直线
		   arc(x,y,半径,开始的弧度,结束的弧度,旋转的方向)//画圆弧
	第四步:fill();//根据画的线型，生成实心图形
	      stroke();//根据画的线型，生成轮廓

(3) 三角形

三角形可以通过(2)中的线型生成，其余的几何形状也可以按照同样的方式生成。

eg:
	var canvas = document.getElementById('canvas');
    var ctx = canvas.getContext('2d');

    // 填充三角形
    ctx.beginPath();
    ctx.moveTo(25,25);
    ctx.lineTo(105,25);
    ctx.lineTo(25,105);
    ctx.fill();

#### 2. 绘制文本 

(1) 文本

文本有三个属性：font、textAlign、textBaseline

font = "bold 14px Arial"

textAlign = "start/end/left/right/center"

textBaseline = "top/hanging/middle/alphabetic/ideographic/bottom/middle"

fillText("文本",x,y)

strokeText("文本",x,y)

measureText("文本").width 返回指定文本的大小

#### 3. 绘制图像

(1) 获得图像

这里的图像来源可以是一张图片、一个视频帧、另外一个canvas

    var img = new Image();   // 创建一个<img>元素
    img.onload = function(){ //用load时间来保证不会在加载完毕之前使用这个图片：
      ctx.drawImage(img,0,0);
      ctx.beginPath();
      ctx.moveTo(30,96);
      ctx.lineTo(70,66);
      ctx.lineTo(103,76);
      ctx.lineTo(170,15);
      ctx.stroke();
    }
    img.src = 'myImage.png'; // 设置图片源地址

(2) 将图像画在画布中 drawImage(传入的image元素，x，y，width，height)

#### 4. 设置阴影

shadowColor:阴影颜色
shadowOffsetX:沿x轴的阴影偏移量
shadowOffsetY:沿y轴的阴影偏移量
shadowBlur:阴影模糊的像素数

上述四种属性均为上下文的属性。

#### 5. 设置颜色

(1) 直接设置颜色

- fillStyle = "color"  设置填充颜色
- strokeStyle = "color"  设置边框颜色
- globalAlpha = 透明度	设置canvas里所有图形的透明度（使用较少）

(2) 使用渐变设置颜色

渐变分为两种：线性渐变、径向渐变

第一步：为图形创建一个渐变对象

    createLinearGradient(startX,startY,endX,endY);//创建线性渐变对象
    createRadialGradient(x1,y1,r1,x2,y2,r2);//创建径向渐变对象

第二步：给渐变对象添加颜色 addColorStop(position,color)

    var LG1 = ctx.createLinearGradient(0,0,150,150);
    LG1.addColorStop(0,'white');
    LG1.addColorStop(1,'black');

第三步：将渐变对象赋给图形的fillStyle、strokeStyle属性

	ctx.fillStyle = LG1;

#### 6. 设置线型样式

lineWidth  设置线条宽度

lineCap   设置线条末端样式 butt，round 和 square

lineJoin  设定线条与线条间接合处的样式 round, bevel 和 miter

miterLimit 限制当两条线相交时交接处最大长度

getLineDash() 返回一个包含当前虚线样式，长度为非负偶数的数组

setLineDash() 设置当前虚线样式

lineDashOffset 设置虚线样式的起始偏移量


<font color="red">暂时只总结了最最基本的用法，后面的用法以后有时间再整理一下。</font>

### 三. canvas中的width、height与一般style中的width、height的区别

1. **canvas标签的width、height**

canvas标签的width和height是画布实际宽度和高度，绘制的图形都是在这个上面，绘制图形的坐标也是以canvas的左上角为起点。

canvas的width、height只能设置数字宽度和高度，不能设置百分比。

canvas的默认宽度为300px,默认高度为150px

2. **canvas作为html的标签，所以也有style属性中的width、height，style的width和height是canvas在浏览器中被渲染的高度和宽度，也就是在浏览器中看起来的高度**

可以通过设置canvas的style.width、style.height来对canvas画布进行缩放。

例如：

	<canvas id="diagonal1" style="border:1px solid;" width="100px" height="100px"></canvas>
    <canvas id="diagonal2" style="border:1px solid;width:200px;height:200px;" width="100px" height="100px"></canvas>
    <canvas id="diagonal3" style="border:1px solid;width:200px;height:200px;" ></canvas>

	<script type="text/javascript">
            function drawDiagonal(id){
                var canvas=document.getElementById(id);
                var context=canvas.getContext("2d");
                context.beginPath();
                context.moveTo(0,0);
                context.lineTo(100,100);
                context.stroke();
            }

            window.onload=function(){                
                drawDiagonal("diagonal1");
                drawDiagonal("diagonal2");
                drawDiagonal("diagonal3");              
            }
    </script>

最终在浏览器中显示的情况如下所示：
[http://panpanfish.com/myDemo/canvas-width.html](http://panpanfish.com/myDemo/canvas-width.html)

diagonal1的style.width、style.height比例与画布相等，因此没有什么影响

diagonal2的style.width、style.height比例与画布比例一致，因此将画布等比例放大了

diagonal3的style.width、style.height比例与画布比例不一致，因此上面的线的位置发生了变化。遵循的一个原则是，保持相对为置不变：(100,100)在(300,150)的画布上占的位置是x轴1/3，y轴2/3的那个点，当canvas的渲染高度宽度变成了(200,200)时，线的终点变成了(200 * 1/3,200 * 2/3)

### 四. canvas移动端屏幕适配问题

在移动端，可以根据百分比布局来适配canvas画布。

可以根据`document.documentElement.clientWidth`来获取屏幕的宽度，将宽度赋给`canvas.width`（如果画布要满屏的话，其他情况也同理），根据定好的长宽比，算出高度，将高度值赋给`canvas.height`

例如：

	var clientWidth = document.documentElement.clientWidth;//获得设备可见区域宽度
	canvas.width = clientWidth;
	canvas.height = clientWidth*800/600;

	然后在画布相应的位置上画上相应的图形即可



