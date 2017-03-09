---
layout: post
title: ECMAScript 6入门
date: 2016-07-31
categories: javaScript
tags: [javaScript,ES6]
description: 
---

### 一. 什么是ECMAScript?与javaScript有什么关系？

ECMA————国际标准化组织

ECMAScript是针对javaScript语言的制定的标准。

ECMAScript是javaScript的规格，javaScript是ECMAScript的实现。

随着时间的推移，浏览器对ES6中的支持已经越来越高，大部分特效已经实现了。

### 二. Babel

由于现在的浏览器对ES6的支持还没那么好，所以引入ES6代码之前需要对其进行转码。babel就是一个将ES6代码转为ES5代码的转码器。

### 三. ES6中的新特性

#### 1. ES6中新增的声明变量的方法

ES6中新增了2种声明变量的方法：**let**和**const**

使用let和const声明的变量，都是在**块级作用域**内有效，不存在变量提升，且不能被重复声明。

##### (1). 块级作用域｛｝

ES6中增加了块级作用域的概念，块级作用域的特点：

- 外层作用域无法读取内层作用域的变量
- 外层代码块不受内层代码块的影响

##### (2). let命令

let命令和var命令一样，都是用来声明变量的

**let与var不同之处：**

- let所声明的变量只在块级作用域内有效，而var声明的变量为函数作用域
- let声明的变量在相同作用域内，不能重复声明
- let不存在变量提升
- 存在暂时性死区：在该作用域内，在使用let声明变量之前，是不能使用该变量的。
- var声明的变量是全局变量的属性，let声明的变量不是全局变量的属性

##### (3). const命令

const命令用来声明常量，其特点如下：

- 若声明的常量是基本类型的常量，声明之后其值不能改变；若声明的常量是复合类型的常量，声明之后其地址不能改变。
- 不能重复声明
- 声明常量时需要立即初始化
- 只在块级作用域内有效

```
//当声明的常量是一个数组时，其地址不能改变，但是其值可以改变
const a = [];
a.push('Hello'); // a的值可以改变
a.length = 0; // 可执行
a = ['Dave']; // 不能将一个地址赋值给a

//若需要声明一个不能修改的对象常量，使用Object.freeze()方法
const foo = Object.freeze({ a: 2});

Object.freeze()是用来冻结一个对象的
```

##### (4). 使用const和let声明变量的好处

- 使用let可以代替ES5中的自执行函数，块级作用域
- 避免由变量声明提升带来的一些问题

#### 2. for-of循环

遍历数组的几种方法：

```
//最原始的方法
for(var index=0; index < myArr.length; index++) {
	console.log(myArr[index]);
}

//使用forEach，这种方法的缺点：不能使用break语句中断循环，也不能使用return语句返回到外层函数
myArr.forEach(function(item,index,arr){
	console.log(item);
});

//使用for-in，这种方法的缺点是会遍历所有的可枚举的属性，这里的index指的是字符串
for(var index in myArr) {
	console.log(myArr[index])
}
```

上面三种做法都有缺点，ES6提出了一种新的遍历数组的方法for-of，**for-of循环语句通过方法调用来遍历各种集合**。

**使用for-of遍历数组**

```
for (var i of [1,2,3]) {
  console.log(i);
}

输出：
1 
2
3
```
**使用for-of遍历字符串**

```
for (var i of 'qwe') {
  console.log(i);
}

输出：
q
w
e
```

for-of用来遍历ES6中的一些新的对象（有迭代器方法的对象）：

数组、类数组对象、Map、Set、Generator、字符串

**什么是遍历器对象？**

有next方法的对象，就具备了遍历器功能。
next方法必须返回一个包含value和done两个属性的对象。
value属性是当前遍历的位置的值，而done属性是一个布尔值，用来表示遍历是否结束。

eg:

```
	
	function iterator() {
		var index = 0;
		return {
			next: function() {
				return {value: index++, done: false}	
			}
		}
	}

	var it = iterator();
	it.next().value; // 0
	it.next().value; // 1
	it.next().value; // 2

	for(var n of it) {
		if(n > 5) {
			break;
		}
		console.log(n);
	}
	
	控制台：0 1 2 3 4 5
```



#### 3. 函数的扩展

##### （1）. 箭头函数：=>
```
  param => {
    statements
  }
```
**箭头函数的特点：**

- 若箭头后面是一个表达式，那么这个表达式的值会被return
- 若箭头后面是一个代码块，那么需要用return语句来给函数返回值
- 箭头函数不能当成构造函数使用
- 箭头函数内部不能使用arguments对象

**与ES5中的函数写法相比：**

解决了this的指向问题

- **在ES5中：**this对象的指向与函数具体是在什么样的情境下使用有关（一共有4种情况，普通函数，构造函数，对象的方法，apply/call/bind）
- **在ES6中：**箭头函数中this的指向是固定的，箭头函数内的this值指向绑定时所在对象，而不是使用时。

##### （2）. 可以给函数参数设置默认值
```
	function log(x,y='world') {
  		console.log(x,y);
	}
```

**与ES5的写法相比：**

- 在ES5中：以前给参数设默认值，var y = y || 'world';
- 在ES6中：给函数参数设置默认值的这种方式更简便一些。

##### （3）. 引入不定参数（形式为‘...变量名’）

不定参数是一个数组，将多余的参数放在了该数组中。

在所有函数参数中，只有最后一个才可以被标记为不定参数。

若没有多余的参数，那么不定参数是一个空数组。

可以代替ES5中arguments变量，arguments是一个类数组对象。

**与ES5的写法相比：**

- **在ES5中：**之前我们需要使用函数的参数时，需要将arguments类数组对象转换成一个数组。

Array.prototype.slice.call(arguments)

- **在ES6中：**就可以直接使用不定参数了。

##### （4）. 扩展运算符 ...

作用：将一个**数组**转为**用逗号分隔的参数序列**。

```
console.log(...[1,2,3]); //1 2 3
```

**与ES5相比：**在ES5中需要使用apply函数才能将数组转换成函数的参数。

举个栗子：将两个数组合并

```
  //在ES5中
  var a = [1,2,3];
  var b = [4,5,6];
  Array.prototype.push.apply(a,b);
  
  //使用ES6的扩展运算符
  a.push(...b)
```
#### 4. 生成器函数Generator

协程：多个线程互相协作，完成异步任务。

（1）generator函数的定义          

`function* myFunc () {  }`

（2）generator函数的返回值，是一个遍历器对象

```
	function* helloWorld () {
	    yield 'hello';
	}

	var hw = helloWorld();//返回一个遍历器对象
	
	//执行遍历器对象的next方法，会返回一个对象，移动内部指针，即执行异步任务的第一阶段，遇到yeild会停止
 	var g = hw.next(); //g = {value: 'hello', done: bool};
```

遍历器对象有两个属性：value和done。

- value 属性是 yield 语句后面表达式的值，表示当前阶段的值；
- done 属性是一个布尔值，表示 Generator 函数是否执行完毕，即是否还有下一个阶段

在函数外面可以使用**g.throw()**捕获错误

(3). 调用Generator函数时，函数并不执行，而是返回一个指向内部状态的指针对象——遍历器对象。



(4). Generator中yield语句和return语句

相同点：执行yield语句和return语句都能返回紧跟在语句后的值

不同点：一个Generator函数中只能出现一次return语句，但是可以出现多次yield语句。

#### 5. promise对象

是js中的一种**异步编程**的模式。

**与回调函数的不同点：**

将回调函数变成了链式写法，每一个异步任务返回一个Promise对象，该对象有一个then方法。

promise对象保存一个异步操作的结果。

**（1）promise对象的三种状态**

- pending进行中
- resolved已完成
- rejected已失败

对象状态不会被外部改变，由异步操作的结果确定

对象状态发生改变了之后就不会再改变了

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数

#### 6. 模版字符串

使用反撇号` `` `

模板字符串内部使用`${}`将js表达式包起来

与一般字符串的不同点：支持多行书写，比`+`号更优雅

#### 7. 解构赋值

#### 8. symbol类型

symbol是js中新增的一种数据类型。

作用：可用作唯一的属性键，用来标识状态

举个栗子：

```
	// isMoving是一个Symbol对象
    var isMoving = Symbol("isMoving");
    ...
    if (element[isMoving]) {
      smoothAnimations(element);
    }
    element[isMoving] = true;
```

#### 9. class类

##### 1. 定义class

class关键字可以定义类

**ES5中通过构造函数来定义对象**

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

**ES6中通过 class关键字定义类**

```
//定义类
class Point {
//ES6中的构造方法constructor方法相当于ES5中的构造函数Point()
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

//使用类，与ES5的方法一致

```
 var a = new Point();
```

注意：(1). 类里面的构造函数之间不需要逗号分隔
(2). 用class声明的类的数据类型就是函数
(3). 类里面定义的所有的方法都试定义在类的原型(prototype)上
(4). 一个类必须有constructor方法
(5). 不存在变量提升
(6). class 与function类似，可以使用表达式的形式定义
              ```
 const MyClass = class { /* ... */ };
              ```
(7). 用_fn的命名方式，定义私有方法

##### 2. class的继承

通过关键字extends实现继承

```
//ColorPoint继承了Point的所有属性和方法
class ColorPoint extends Point {}
super(); //调用父类的构造函数方法
子类必须在constructor方法中调用super方法
```

##### 3. class的取值函数和存值函数

对某个属性设置取值函数和存值函数

```
class MyClass {
  constructor() {
    // ...
  }
  //prop属性的取值函数
  get prop() {
    return 'getter';
  }
  //prop属性的存值函数
  set prop(value) {
    console.log('setter: '+value);
  }
}
```

##### 4. class的静态方法

(1). 使用`static`关键字声明的静态方法不会被实例继承
(2). 父类的静态方法可以被子类调用
(3). 子类的静态方法可以被super对象调用

##### 5. class的静态属性和实例属性
class的静态属性：指的是class本身的属性，而不是定义在实例对象上的属性。

```
eg:
class Foo {
   static prop = 1;
}
class的实例属性，可以用等式，写入类的定义之中。
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp); // 42
  }
}
```

ES6中的class解决的问题：

#### 10. ES6中新增的模块系统

export
import

在ES6中，无论是否加入'use strict;'语句，默认情况下都是在严格模式下进行的。

##### 1. 导出

用export声明的部分，可以供其他模块使用。

只写一行你想要导出的变量列表，再用花括号包起来。
```
export {module1, module2...}
```

默认导出 
```
export default ...
```
export default命令用于指定模块的默认输出。一个模块只能有一个默认输出。

本质上，export default就是输出一个叫做default的变量或方法，在导入时，可以为其取任意名字。

在ES6中，使用CommonJS、AMD模块都有一个默认导出

```
//下面两种写法是等价的
var colors = require('colors.js');
import colors from colors.js;
```

##### 2. 导入
```
import {module1, module2} from "other.js";
improt module form "other.js";
```

##### 3. 给导入或者导出的变量重新命名，使用as

```
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```