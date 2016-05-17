---
layout: post
category: DesignPattern
title: 设计模式之静态代理模式
tagline: by zhanyr
tags: [DesignPattern]
---

代理模式是对象的结构模式。代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。

<!--more-->

### 概述
	
代理模式是一个类代替另一个类采取行动。在一些情况下，某个对象不能直接引用另一个对象，代理对象可以在客户端与目标对象之间起到中介的作用。本文讲的时一种简单代理模式。

### 类图

![代理模式类图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/proxy.png)

其中：

抽象接口角色AbstractIntf:声明了目标对象和代理对象的共同接口，只有他们实现共同的接口，才能在任何使用目标对象的地方使用代理对象。

目标对象角色RealClass:代理代表的目标对象。

代理对象角色ProxyClass:代理对象内部含有目标对象的引用，从而可以在任何时候操作目标对象。代理对象通常在客户端调用传递给目标对象之前或之后，执行某个操作，而不是单纯的将调用传递给目标对象。如，代理对象的方法中可以调用目标对象的方法，并且在方法的前后加上日志等内容。


### 代码实现

AbstractIntf.java

	public interface AbstractIntf {
		public void method();
	}

RealClass.java
	
	public class RealClass implements AbstractIntf{
		@Override
		public void method() {
			System.out.println("RealClass method");
		}
	}
	
ProxyClass.java

	public class ProxyClass implements AbstractIntf{
		private AbstractIntf real;
		public ProxyClass(){
			this.real = new RealClass();
		}
		public ProxyClass(AbstractIntf real){
			this.real = real;
		}
		@Override
		public void method() {
			System.out.println("use proxy");
			real.method();
		}
	}
	
ProxyTest.java
	
	public class ProxyTest {
		@Test
		public void testProxy(){
			AbstractIntf obj = new ProxyClass(new RealClass());
			obj.method();
		}
	}
	
结果:

use proxy
RealClass method

### 举个🌰

我要回家，没空去火车站买票，这时候就可以找票贩子去买。我和票贩子实现同样的买票接口，都拥有买票方法，票贩子拿着我的证件就可以帮我买到回家的车票。这就是一种典型的代理模式。代码略
	
### 总结

**设计思想：**

通过为对象加一个代理来降低对象的使用复杂度、或是提升对象使用的友好度、或是提高对象使用的效率。

**优点：**

1. 职责清晰：真实的角色就是实现实际的业务逻辑，不用关心其他非本职责的事务，通过后期的代理完成一件完成事务，附带的结果就是编程简洁清晰。2. 
2. 保护目标对象：实际对象不直接操作目标对象，而是由代理进行操作，这样就起到了保护目标对象的作用。
3. 扩展性好

**缺点：**

1. 在客户端和目标对象增加一个代理对象，会造成请求处理速度变慢。
2. 增加了系统的复杂度。

**适用场景：**

1. 远程代理，也就是为一个对象在不同的地址空间提供局部代表。这样可以隐藏一个对象存在于不同地址空间的事实。
2. 虚拟代理，根据需要创建开销很大的对象。通过它来存放实例化需要很长时间的对象。
3. 安全代理，用来控制真实对象访问时的权限。
4. 智能指引，当调用目标对象时，代理可以处理其他的一些操作