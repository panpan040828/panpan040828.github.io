---
layout: post
title: 数据结构与算法——javaScript
date: 2016-08-28
categories: 算法学习
tags: [读书笔记,算法]
description: 
---

### 一. 数组

js中的数组的一些基本方法见博客<http://>

#### 1. 二维数组

#####  (1). 二维数组的创建

```
function matrix(row,col,init) {
	var arr = [];
	for(var i = 0; i < row; i++) {
		arr[i] = [];
		for(var j = 0; j < col; j++) {
			arr[i][j] = init;
		}
	}
	return arr;
}

matrix(2,3,0);
```

##### (2). 访问二维数组

**按行访问**

```
var grade = [ [11,12,13],
			  [21,22,23],
			  [31,32,33] ];
for(var row = 0; row < grade.length; row++) {
	for(var col = 0; col < grade[row].length; col++) {
		console.log(grade[row][col]);
	}
}
```
**按列访问**

```
var grade = [ [11,12,13],
			  [21,22,23],
			  [31,32,33] ];
for(var col = 0; col < grade.length; col++) {
	for(var row = 0; row < grade[col].length; row++) {
		console.log(grade[row][col]);
	}
}
```
#### 2. 对象数组、数组对象

**对象数组**

数组中的元素是对象

`eg: [{a: 1},{b: 2}]`

**数组对象**

对象中的属性是数组

`eg: {a: [1,2,3]}`

### 二. 列表

当不需要在一个很长的序列中查找元素，或者对其进行排序时，可以使用列表。

### 三. 栈

#### 1. 栈是一种特殊的列表，只能通过一端访问。后入先出。

function Stack() {
	this.dataStore = []; //存放栈中的元素
	this.top = 0; //栈顶的位置 ＋ 1
	this.push = push; //入栈的函数
	this.pop = pop; //出栈的函数
	this.peek = peek; //返回栈顶的元素
}

#### 2.使用栈的例子

**数制间的相互转换**

输入的数字是n，转换为以b为基数的数字

- 最低位是`n%b`，将此位压入栈
- 使用`Math.floor(n/b)`代替之前的n再次输入
- 重复前2个步骤，直到`Math.floor(n/b)`为0，则不再循环

```
function mulBase(num, base) {
	var stack = [];
	
	//将每一次求得得余数入栈
	do {
		stack.push(num % base);
		num = Math.floor(num / base);
	} while (num > 0)
	
	var result = '';
	
	//依次出栈
	while(stack.length > 0) {
		result += stack.pop();
	}
	
	//返回输出的结果
	return result;
}
```

**回文**

字符串反转之后是否与原字符串相等，如果相等，则为回文

使用栈的思想，首先将字符串依次入栈，再依次出栈，判断两个字符串是否相等

<font color='red'>注意：js中字符串没有反转操作的函数，而数组有arr.reverse()</font>

```
function isHuiWen(str) {
	var arr = str.split();
	var reverseStr = '';
	while(arr.length > 0) {
		reverseStr += arr.pop();
	}
	
	if(reverseStr == str) {
		return true;
	} else {
		return false;
	}
	
}
```

### 四. 队列

先入先出

function Queue() {
	this.dataStore = []; //存放队列中元素的数组
	this.enqueue = enqueue; //入队列，队尾增加元素
	this.dequeue = dequeue; //出队列，队头删除元素
	this.front = front;
	this.back = back;
	this.toString = toString;
	this.empty = empty;
}

