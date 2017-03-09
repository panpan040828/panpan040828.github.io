---
layout: post
title: javaScript 继承问题
date: 2016-03-22
categories: javaScript
tags: [javaScript]
description: 
---

最近在看《javaScript高级程序设计》，就其中的继承写一下自己的理解。

### 一. 什么是继承

继承是指一个对象直接使用另一对象的属性和方法。

js中继承的方法有：原型链继承、构造函数继承、组合继承、原型式继承、寄生式继承、寄生式组合继承

#### 1. 原型链继承

#### （1）. 原型、构造函数和实例的关系

```
	function Person() {		
	}

	Person.prototype 

	var p = new Person();

	构造函数：Person            属性：prototype => 指向原型对象
	原型对象：Person.prototype  属性：constructor => 指向构造函数
									__proto__ (原型指针) => 指向原型链上一层的原型对象 
	实例：p                     属性：__proto__(原型指针) => 指向原型对象
```

具体如下图所示：

![原型、构造函数和实例的关系](/uploads/post/demo/prototype.jpg)

一个问题：

```
	  function Person(name,age) {
	  	this.name = name;
	  	this.age = age;
	  }

	  Person.prototype = {
	  	getAge: function() {
	  		alert(this.age);
	  	}
	  }

	  var p = new Person('lucy',12);
	  console.log(p.constructor === Person);//false
	  console.log(p.constructor);//Object
```

出现上面那个结果的原因是，实例是没有constructor这个属性的，p.constructor其实是访问的p.prototype.constructor，但是由于我们用对象字面量的方式，重写了Person.prototype，所以会顺着原型链往上找，找到Object.prototype.constructor


#### （2）. 原型链

如（1）中所示，如果让（1）中的原型对象成为另外一个类型的实例，以此类推，便构成了原型链。

<font color="red">注意：所有构造函数的默认原型都是Object的实例，因此默认原型会有一个prototype指针指向object.prototype。</font>

#### （3）. 原型链继承

主要思想是：通过设定一种类型的原型是另一种类型的实例来继承另外一种类型。

    例如：B要继承A
    
    创建A的实例：new A();
    让其等于B的原型：B.prototype = new A();//B则继承了A里面所有的属性和方法

**原型搜索机制：**以读取方式访问一个实例属性时，若在该实例中搜索该属性，没有找到再搜索实例的原型，顺着原型链往上找。

代码示例如下：

     function Parent(){
    	this.colors = ["red"];
    }

    function Child() {
	
	}

    Child.prototype = new Parent();/*child的原型是parent的实例*/
    /*实现了child继承parent*/
    var A = new Child();
    console.log(A.colors); //['red']

原型继承的缺点：

（1）.超类型中有引用类型的属性时，子类型继承超类型，一个子类型中对该引用类型修改，会反应到所有的子类型上。所有子类型共享超类型的引用类型

例如：
```
		function A() {
	    	this.colors = ['red'];
	    }
	    function B() {

	    }
		
		//B继承了A
	    B.prototype = new A();	

		//B的两个实例    
	    var example1 = new B();
	    var example2 = new B();

		//第一个实例修改了colors属性
	    example1.colors.push('blue');

	    console.log(example2.colors); //['red','blue']
	    console.log(example1.colors); //['red','blue']
```

（2）.创建子类型实例时无法向超类型的构造函数中传递参数


#### 2. 构造函数继承

利用call()和apply()方法在新的对象上执行超类型的构造函数。

超类型：指的是被继承的类型。

    a.func.call(b)  /*指的是a对象的方法应用到b对象上*/
    a.func.apply(b)

代码示例：

    function parent(){
    	this.colors = ["red"];
    }
    
    function child(){
    	parent.call(this); /*或者 parent.apply(this);*/
		/*实现了child继承parent*/
    }
    
    var one = new child();

**构造函数继承的优点：**

使子类child在创建对象的同时传递参数到父类parent

**构造函数继承的缺点：**

无法进行函数的复用，a的原型中定义的方法也无法继承。

#### 3. 组合继承(将原型链继承和借用构造函数继承组合起来)

主要思想是：使用原型链实现对a原型属性和方法的继承，通过构造函数实现对a实例属性的继承。

代码示例如下：

    function Parent() {       
	    this.sayAge=function() {  
	    	console.log(this.age);  
	    }  
    }  
      
    Parent.prototype = {
		sayParent: function() {  
       		alert("this is parentmethod!!!");  
    	}  
	}

      
    function Child(firstname) {  
	    Parent.call(this);//构造函数继承  
	    this.fname=firstname;  
	    this.age=40;  
	    this.saySomeThing = function() {  
		    console.log(this.fname);  
		    this.sayAge();  
	    }  
    }  
      
    Child.prototype = new Parent();//原型链继承
    var child = new Child("张");  
    child.saySomeThing();  
    child.sayParent();

缺点：要调用两次超类型的构造函数  

#### 4. 原型式继承

主要思想是：实现对父类的**浅复制**，产生一个副本

Object.create()函数实现的就是原型式继承

代码示例如下：
	
    function object(object) {
    	function F(){} //一个构造函数F
    	F.prototype = object; //将对象赋值给构造函数F的原型，引用类型赋值
    	return new F(); //返回一个F的实例
    }

    var parent = {
    	colors: ["red"]
    }
    
    var child = object(parent); /*实现了child继承parent*/
	child.colors.push("blue");
	console.log(parent.colors); //["red", "blue"]
	console.log(child.colors); //["red", "blue"]
	
这种继承方式是将child的原型设为parent，通过child可以访问parent的属性和方法，而不需要将同名属性和方法在child里面再重新定义一遍。

**Object.create()**

可接收两个参数，第一个参数是需要继承的对象，第二个参数是新生成的对象中新增加的属性和方法。

```

	var parent = {
		name: 'a',
		sayName: function() {
			console.log(this.name)
		}
	}

	var child = Object.create(parent,{
		name: {writable:true, configurable:true, value: 'b'}
	});

	parent.sayName(); // 'a'
	child.sayName(); // 'b'
```

#### 5. 寄生式继承

主要思想：寄生式继承是基于原型式继承，创建一个仅用于封装继承的函数

代码示例如下：
	
    function object(object) {
    	function F(){}//一个构造函数F
    	F.prototype = object;//将对象赋值给构造函数F的原型，引用类型赋值
    	return new F();//返回一个F的实例
    }

	function createAnother(original) {
		var clone = object(original);
		clone.sayHi = function() {
			alert("Hi!");
		};
		return clone;
	}

    var parent = {
    	colors: ["red"];
    }
    
    var child = createAnother(parent);/*实现了child继承parent*/
	child.colors.push("blue");
	console.log(parent.colors);//["red", "blue"]
	console.log(child.colors);//["red", "blue"]

#### 6. 寄生组合式继承（将寄生式继承和组合继承的方式结合起来）

**组合式继承的缺点：**会调用两次超类型的构造函数，会在子类型的原型和实例中创建2次超类型构造函数中的属性（实例中的同名属性会覆盖原型中的）

YUI框架的`extend`函数就是使用的`寄生组合式继承`。

    //Sub是子类型的构造函数，Super是超类型的构造函数
    function extend(Sub,Super) {
    
	    //引入空函数,避免创建父类的新实例（效率比较高）
	    var F = function() {};
	    F.prototype = Super.prototype;
	    
	    //使用新函数创建实例而不是父类型创建
	    Sub.prototype = new F();
	       
	    Sub.prototype.constructor = Sub;
	    
	    //提供Super属性,弱化父子类间耦合
	    Sub.super=Super.prototype;

	    if(Super.prototype.constructor == Object.prototype.constructor) {
	    	Super.prototype.constructor = Super;
	    }
    }

	//父类型
	function Parent(name) {
		this.name = name;
		this.colors = ["red","blue","green"];
	}

	Parent.prototype = {
		sayName: function() {
			alert(this.name);
		}
	}

	//子类型
	function Child(name,age) {
		//调用超类型的构造函数
		Parent.call(this,name);
		this.age = age;
	}
	
	//使用extend函数来代替组合继承里面的创建父类型的实例
	extend(Child,Parent);


### 二. 如何判断一个属性在实例中还是在原型中

instance.hasOwnProperty('属性名')

```
function hasPrototypeProperty(object,name){
	return (name in object) && !object.hasOwnProperty(name)
 }
```