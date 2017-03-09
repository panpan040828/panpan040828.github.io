---
layout: post
title: CSS3 flexbox弹性布局
date: 2016-03-25
categories: 前端笔记
tags: [css]
description: 
---

使用过bootstrap框架之后，觉得它最大的一个优点就是它的栅格布局，使用栅格布局使得布局变得简单了很多，不用去考虑一些float、position属性。

flexbox是一种与栅格布局类似的方式。

是2009年w3c提出的一种布局方式，也叫做弹性布局，弹性布局的出现给盒模型提供了很大的便利性。

### 一. 如何让容器变成flexbox布局

#### 1. 父元素的设置
    .parent{
      display: -webkit-flex; /* Safari */
      display: flex;
    }
    
	或者成为行内flex布局

    .parent{
      display: inline-flex;
    }

<font color="red">注意：设为flexbox布局之后，子元素的**float**、**clear**、**vertical-align**属性都会失效。</font>

#### 2. 子元素的设置
    
    .child{
    	flex:[number];
    }
    
给其中的每一个孩子设置 `flex: [number]` 来让他们按比例分配容器的宽度。

比如三个child分别设置了 `flex: 1` `flex: 2` `flex: 1` 则他们是按照 1-2-1 的比例来分配宽度的。

如果有child没有设置 `flex` 而是设置了固定的宽度，比如 `width: 100px` 那么它的宽度就不受flex容器的影响，但是其他的设置了 `flex: [number]` 的容器会按比例平分剩下的部分。


### 二. flex布局的属性

#### 1 . 针对父元素的属性

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

#### (1). flex-direction

决定项目的排列方向（主轴的方向）

     flex-direction: row | row-reverse | column | column-reverse;

按照字面意思也挺好理解的。

#### (2). flex-wrap

决定项目的换行方向

     flex-wrap: nowrap | wrap | wrap-reverse;

- nowrap:不换行
- 
- wrap:正常换行
- 
- wrap-reverse:换行，但是第一行在下方，如下图所示：

![flexbox反方向换行](/img/post/demo/flex-wrap-reverse.jpg)

#### (3). flex-flow

是 flex-direction 和 flex-wrap 组合形式

#### (4). justify-content

定义项目在**主轴**的对齐方式

 `justify-content: flex-start | flex-end | center | space-between | space-around;`

- flex-start（默认值）：在项目开始的地方对齐
- flex-end：在项目结束的地方对齐
- center： 居中
- space-between：两端对齐，项目之间的间隔都相等。
- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

#### (5). align-items

定义项目在**交叉轴**的对齐方式

     align-items: flex-start | flex-end | center | baseline | stretch;

- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- baseline: 项目的第一行文字的基线对齐。
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#### (6). align-content

同时定义项目在**主轴**和**交叉轴**（水平线上）的对齐方式

     align-content: flex-start | flex-end | center | space-between | space-around | stretch;

- flex-start：主轴方向与起点对齐，交叉轴方向也与起点对齐。
- flex-end：主轴方向与终点对齐，交叉轴方向也与终点对齐。
- center：主轴和交叉轴方向均为居中。
- <font color="red">space-between：主轴和交叉轴方向均两端对齐，轴线之间的间隔平均分布。</font>这个有待考证
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- <font color="red">stretch（默认值）：轴线占满整个交叉轴</font>这个有待考证

#### 2 . 针对子元素的属性

- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

#### (1). order

定义项目的排列顺序，数值越小，排列越靠前。

    order:<number>; /* default:0 */

#### (2). flex-grow

定义项目的放大比例。

    flex-grow:<number>; /* default:0 */

#### (3). flex-shrink

定义项目的缩小比例。

    flex-shrink:<number>; /* default:0 */

#### (4). flex-basis

定义项目占据主轴的空间。

    flex-basis:<length>; /* default:auto */

#### (5). flex

flex是flex-grow、flex-shrink、flex-basis的缩写形式

    flex:flex-grow flex-shrink flex-basis; /* default: 0 0 auto */

#### (5). align-self

允许单个子元素与其他子元素在交叉轴上的对齐方式不一样

    align-self:

- auto:保持元素本身的对齐方式
- flex-start：交叉轴的起点对齐。
- flex-end：交叉轴的终点对齐。
- center：交叉轴的中点对齐。
- baseline: 项目的第一行文字的基线对齐。
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。