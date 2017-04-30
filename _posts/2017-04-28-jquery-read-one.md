---
layout:     post
title:      "jQuery源码阅读笔记一"
subtitle:   "当我们使用$()时到底发生了什么"
date:       2017-04-28 14:47:31
author:     "杨"
header-img: "img/post-bg-04.jpg"
---

# jQuery源码阅读笔记一

--- jquery version:3.2.1



#### 当我们使用$()时到底发生了什么

1.$是jQuery的别名，$()和jQuery是等价的

```
 window.jQuery = window.$ = jQuery;
```
2.jQuery构造函数

```
    // Define a local copy of jQuery
	jQuery = function( selector, context ) {

	// The jQuery object is actually just the init constructor 'enhanced'
	// Need init if jQuery is called (just allow error to be thrown if not included)
	return new jQuery.fn.init( selector, context );
},
```
> 在javascript中，构造函数如果有返回值时，运算符 new所创建的对象会被丢弃，返回值将作为new表达式的值，jQuery利用了这一特性，通过在构造函数jQuery()内部用运算符new创建并返回另一个构造函数的实例，省去了构造函数jQuery()前面的new,即我们创建jQuery对象时可以省略new,直接写jQuery()。



3.jQuery.fn和jQuery.prototype 是等价的

```
 jQuery.fn = jQuery.prototype
```
4.既然jQuery()构造函数返回的是jQuery.fn.init()构造函数产生的实例，那么代码中所有挂载在jQuery.fn上的函数，jQuery()创建的实例是不能调用的。因为jQuery(),jQuery.fn.init()两个构造函数的原型不一样产生的实例也是不同的，jQuery是通过下面的代码解决的。

```
// Give the init function the jQuery prototype for later instantiation
init = jQuery.fn.init = function( selector, context, root ) {
    
    
}
init.prototype = jQuery.fn;
```

5.当我们使用$()时就是返回了一个 以jQuery.fn.init( selector, context )为构造函数的对象，这个对象的原型是jQuery.


#### 模仿这个过程的小例子



```
	<script type="text/javascript">
		var Q=function(){
			return new Q.fn.init();
		}

		Q.fn=Q.prototype;
		Q.fn.init=function(){
			return this;
		}
		Q.sayhi=function(){
			console.log('hi');
		}
		Q.fn.sayHello=function(){
			console.log('hello');
		}
		Q.fn.init.prototype=Q.prototype;//如果去掉这一行 下面的qq.sayHello() 无法执行
		Q.sayhi();
		

		var qq =  Q();
		console.log(qq);
		qq.sayHello();

	</script>
```





#### 参考资料
    
    1.jQuery技术内幕：深入解析jQuery架构设计与原理实现 -- 高云