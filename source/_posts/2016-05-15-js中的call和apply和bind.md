---
layout: post
title: js中的call和apply和bind
date: 2016-05-15
categories: javaScript
tags: [javaScript,算法]
description: 
---

apply、call、bind是为了手动绑定this对象，合理利用apply、call和bind会使得javaScript代码更加优雅。

### 一. js中的apply()、call()、bind()

#### 1. apply:应用某一对象的一个方法，用另一个对象替换当前对象。

**apply(obj,参数数组)** 

#### 2. call:与apply的作用一样，只是参数列表不一样

**call(obj,参数1,参数2...)**

#### 3. bind()与call和apply的不同之处在于，bind会返回一个绑定函数

bind()会创建一个新的函数，称为绑定函数，,绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

**bind(obj,参数1，参数2...)**

**举个栗子：**
	```js
	var xw = {
			    name : "小王",
			    gender : "男",
			    age : 24,
			    say : function(a,b) {
			      alert(this.name + a + b);
		    	}
      }
    
    var xh = {
			    name : "小红",
			    gender : "女",
			    age : 18
    }
    
    xw.say();//输出  小王

	如何用xw里面的方法 say 来输出xh里面的name呢
	
	用call()实现
	xw.say.call(xh,"my","call");//  小红mycall

	用apply()实现	
	xw.say.apply(xh,["my","apply"]); 小红myapply

	用bind()实现
	xw.say.bind(xh,"my","bind")();//bind返回的是一个函数
	xw.say.bind(xh)("my","bind");//由于bind返回的是一个函数，因此也可以在调用的时候传入参数
	```

#### 4. js中将类数组对象转化为一般数组

(1) 什么是类数组对象
	```js
	eg:
    var myObj = {
    	"0": 1,
    	"1": 2,
    	"2": 3,
    	"length": 3
    }
	myObj就是一个类数组，myObj是一个对象，此对象将数组的下标作为属性名。
	```

(2) arguments对象是类数组对象

在函数体内，arguments是收到的**实参**副本。（注意是实参，不是形参，由实际接收到的参数决定）
	```js
    eg:
    function myfuc() {
    	console.log(arguments[0]+arguments[1]);
    	console.log(arguments.length);
    }
    
    myfuc("h","d");//控制台打印出 hd 2
	```

(3) 如何将类数组对象转换为一般的数组

利用数组中的slice()方法，此方法会生成一个新的数组。
	```js
    var argArr = Array.prototype.slice.call(arguments,0);

	eg：

	var my_object = {
        '0': 'zero',
         '1': 'one',
         '2': 'two',
         '3': 'three',
         '4': 'four',
         length: 5
     };

    console.log(my_object);//my_object是一个对象
    var a = Array.prototype.slice.call(my_object,0);
    console.log(a);//a是一个数组
	```

#### 5. 函数原型中的bind函数

旧版本中的写法:
	```js
	Function.prototype.bind = function(){ 
	    
	    var fn = this;// bind作为Function的prototype属性，每一个函数都具有bind方法，其中的this指向调用该方法的函数(也即一个函数对象实例)————**作为对象方法调用时，指向该对象**
	    
	    var args = Array.prototype.slice.call(arguments);// 将bind函数传入的实参对象转换成数组
	    
	    var object = args.shift();// 数组的shift方法会移除数组的第一个元素，并返回第一个元素，在bind方法的参数里面，第一个参数就是需要绑定this的对象
)
	    return function(){ //返回一个函数，这个函数的功能如下：
	        
			return fn.apply(object, args.concat(Array.prototype.slice.call(arguments))); 

	        // 这里的Array.prototype.slice.call(arguments)指的是bind返回的那个函数的实参列表，也就是实际执行时bind返回的那个函数时传入的参数
	    }; 
	};
	```

新版本中的写法：
	```js
	function bind(context) {
	    if (arguments.length < 2 && Object.isUndefined(arguments[0]))
	        return this;
	
	    if (!Object.isFunction(this))
	        throw new TypeError("The object is not callable.");
	
	    var nop = function() {};
	    var __method = this, args = slice.call(arguments, 1);
	
	    var bound = function() {
	        var a = merge(args, arguments);
	        // Ignore the supplied context when the bound function is called with
	        // the "new" keyword.
	        var c = this instanceof bound ? this : context;
	        return __method.apply(c, a);
	    };
	
	    nop.prototype   = this.prototype;
	    bound.prototype = new nop();
	
	    return bound;
	}
	```
	
	
```
function bind(fn, self){
  var args = [].slice.call(arguments, 2)
  return function(){
    return fn.apply(self, [].concat.call(args, arguments));
  };
};


function add(a, b){
  return a + b;
};


// add.bind(this)

var fn = bind(add, this, 1)

fn(2); // 3

```


   




