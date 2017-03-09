---
layout: post
title: javaScript中的异步编程方法
date: 2016-09-21
categories: javaScript
tags: [javaScript]
description: 
---

### 一. 回调函数

```
//jQuery中，第一次发起ajax请求
$.ajax({
	...
	success: function(data) {
		
		//第一次ajax请求成功执行第二次ajax请求
		$.ajax({
			...
			success: function(data) {
			
			}
		});
	}
})
```

回调函数的缺点：回调地狱

### 二. 事件监听

### 三. 订阅/发布

### 四. Promise对象

>Promise到底解决什么问题？

>正如上面代码所示，笔者认为，Promise的意义就在于 **then 链式调用** ，它避免了异步函数之间的层层嵌套，将原来异步函数的 **嵌套关系** 转变为便于阅读和理解的 **链式步骤关系** 。

promise对象表示一个异步操作。

#### 1. promise对象的状态

由于promise对象表示的是一个异步操作，因此promise对象的状态有三种：

- pending 进行中
- resolved 已完成
- rejected 已失败

#### 2. 创建promise对象

```
	
	//接收一个函数作为参数，该函数又有两个参数，这两个参数也是函数
	var promise = new Promise(function(resolve, reject){

	});
```

**resolve()函数的作用：**

将Promise对象的状态由pending ——> resolved
resolve()中传入的参数，将会作为后续成功的函数的参数

**reject()函数的作用：**

将Promise对象的状态由pending ——> rejected
reject()中传入的参数，将会作为后续失败的函数的参数

**promise对象一新建之后，传入的函数参数就会立即执行**

```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('Resolved.');
});

console.log('Hi!');

输出的结果为：
Promise
Hi!
Resolved.
```

#### 3. promise实例上的方法

**then()方法：根据promise对象的状态来确定执行的操作**

promise.then(resolve, reject);

then的第一个参数resolve是异步操作成功时执行的函数，第二个参数reject是异步操作失败时执行的函数。

- 在then方法的第一个参数 resolve 中，如果返回一个新的promise对象，那么就可以进行链式调用。
- 在 then方法的 resolve 的函数中，如果返回某个值，该值可以作为后续操作的参数。

例如：

```

		var p1 = new Promise(function(resolve, reject) {
			resolve('第一步完成');
		});

		var p2 = new Promise(function(resolve, reject) {
			resolve('第二步完成');
		});
	
		p1.then(function(data) {
			console.log(data);
			return p2;
		}).then(function(data) {
			console.log(data);
		});

		控制台输出：
		第一步完成
		第二步完成
```

**catch()方法：相当于then()第二个参数的简单写法**

promise.then(null,function)
promise.catch(reject)


**Promise.all()**

多个异步任务被触发时，要在所有的异步任务都完成之后才做出回应。

Promise.all()的参数是一个promise对象组成的数组，在这些promise对象**全部resolved之后**触发回调函数；或者只有一个promise失败时会触发catch()

**使用场景：在执行多个请求之后再做一些操作。**

```

	var req1 = new Promise(function(resolve, reject) { 
    	// A mock async action using setTimeout
    	setTimeout(function() { resolve('First!'); }, 4000);
	});

	var req2 = new Promise(function(resolve, reject) { 
    	// A mock async action using setTimeout
    	setTimeout(function() { reject('Second!'); }, 3000);
	});

	Promise.all([req1, req2]).then(function(results) {
    	console.log('Then: ', one);
	}).catch(function(err) {
    	console.log('Catch: ', err);
	});

	// From the console:
	// Catch: Second!
```

**Promise.race()**

只要数组中有一个promise对象被resolved或者rejected就会触发。

使用场景：在有两个请求资源时可以使用。

```

	var req1 = new Promise(function(resolve, reject) { 
    	// A mock async action using setTimeout
    	setTimeout(function() { resolve('First!'); }, 8000);
	});

	var req2 = new Promise(function(resolve, reject) { 
    	// A mock async action using setTimeout
    	setTimeout(function() { resolve('Second!'); }, 3000);
	});

	Promise.race([req1, req2]).then(function(one) {
    	console.log('Then: ', one);
	}).catch(function(one, two) {
    	console.log('Catch: ', one);
	});

	// From the console:
	// Then: Second!
```

**Promise.reject(reason)返回一个失败的promise对象**

**Promise.resolve(value)返回一个成功的promise对象**

#### 5. 实现Promise对象

**Promise对象的属性：**

doneList：数组，存放异步操作成功时的回调函数队列
failList：数组，存放异步操作失败时的回调函数队列
state：保存当前Promise对象的状态

**Promise对象的方法：**

then：分别向doneList和failList添加回调函数
catch：向failList中添加失败的回调函数
always：
resolve：将状态改为resolved，并触发绑定的所有成功的回调函数
reject：将状态改为rejected，并触发绑定的所有失败的回调函数

**静态方法:**

Promise.all：参数是多个promise对象组成的数组，

```

	var Promise = function() {
    	this.doneList = [];
    	this.failList = [];
    	this.state = 'pending';
	};

	Promise.prototype = {
    	constructor: 'Promise',

    	resolve: function() {
        	this.state = 'resolved';
        	var list = this.doneList;
        	for(var i = 0, len = list.length; i < len; i++) {
            	list[0].call(this);
            	list.shift();
        	}
    	},

    	reject: function() {
        	this.state = 'rejected';
        	var list = this.failList;
        	for(var i = 0, len = list.length; i < len; i++){
            	list[0].call(this);
            	list.shift();
        	}
    	},

    	done: function(func) {
        	if(typeof func === 'function') {
            	this.doneList.push(func);
        	}
        	return this;
    	},

    	fail: function(func) {
        	if(typeof func === 'function') {
            	this.failList.push(func);
        	}
        	return this;
    	},

    	then: function(doneFn, failFn) {
        	this.done(doneFn).fail(failFn);
        	return this;
    	},

    	always: function(fn) {
        	this.done(fn).fail(fn);
        	return this;
    	}
	};

	function when() {
    	var p = new Promise();
    	var success = true;
    	var len = arguments.length;
    	for(var i = 0; i < len; i++) {
        	if(!(arguments[i] instanceof Promise)) {
            	return false;
        	}
        	else {
            	arguments[i].always(function() {
                	if(this.state != 'resolved'){
                    	success = false;
                	}
                	len--;
                	if(len == 0) {
                    	success ? p.resolve() : p.reject();
                	}
            	});
        	}
    	}
    	return p;
	}
```

```
			/*
            我们要满足状态只能三种状态：PENDING,FULFILLED,REJECTED三种状态，且状态只能由PENDING=>FULFILLED,或者PENDING=>REJECTED
            */
            var PENDING = 0;
            var FULFILLED = 1;
            var REJECTED = 2;

            /*
            value的值为异步操作成功之后，传入resolve函数的参数；或者失败时，传入reject函数的参数。
			deffered保存着状态改变之后的需要处理的函数以及promise子节点，构造函数里面应该包含这三个属性的初始化
             */
            function Promise(callback) {
				
				//status属性存放的是promise对象的状态
                this.status = PENDING;
                this.value = null;
                this.defferd = [];

				//bind()函数返回的是一个匿名函数，callback函数的两个参数是resolve、reject函数
                setTimeout(callback.bind(this, this.resolve.bind(this), this.reject.bind(this)), 0);
            }
            
            Promise.prototype = {
                constructor: Promise,

                //将promise状态改变为FULFILLED
                resolve: function (result) {
                    this.status = FULFILLED;
                    this.value = result;
                    this.done();
                },

                //将promise状态改变为REJECTED
                reject: function (error) {
                    this.status = REJECTED;
                    this.value = error;
                },

                //处理defferd
                handle: function (fn) {
                    if (!fn) {
                        return;
                    }

                    var value = this.value;
                    var t = this.status;
                    var p;

                    if (t == PENDING) {
                         this.defferd.push(fn);
                    } else {

                        if (t == FULFILLED && typeof fn.onfulfiled == 'function') {
                            p = fn.onfulfiled(value);
                        }

                        if (t == REJECTED && typeof fn.onrejected == 'function') {
                            p = fn.onrejected(value);
                        }

                    	var promise = fn.promise;

                    	if (promise) {
                        	if (p && p.constructor == Promise) {
                            	p.defferd = promise.defferd;
                        	} else {
                            	p = this;
                            	p.defferd = promise.defferd;
                            	this.done();
                        	}
                    	}
                    }
                },

                //触发promise defferd里面需要执行的函数
                done: function () {
                    var status = this.status;
                    if (status == PENDING) {
                        return;
                    }
                    var defferd = this.defferd;
                    for (var i = 0; i < defferd.length; i++) {
                        this.handle(defferd[i]);
                    }
                },

                /*储存then函数里面的事件
                返回promise对象
                defferd函数当前promise对象里面
                */
                then: function (success, fail) {
                   var o = {
                        onfulfiled: success,
                        onrejected: fail
                    };
                    var status = this.status;
                    o.promise = new this.constructor(function () {
            
                    });
                    if (status == PENDING) {
                        this.defferd.push(o);
                    } else if (status == FULFILLED || status == REJECTED) {
                        this.handle(o);
                    }
                    return o.promise;
                }
            };
```