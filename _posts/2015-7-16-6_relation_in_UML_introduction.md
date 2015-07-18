---
layout: post
category: Tools
title: UML类图中的六大关系
tagline: by zhanyr
tags: [UML]
---

UML(unified modeling language,标准建模语言)类图用来描述系统中类的静态结构，不仅用来定义系统中的各种类，表示类之间的联系如继承、关联、依赖、聚合等，也包括类的内部结构，包括类的属性和操作。

<!--more-->

##依赖(Dependency)

**含义：**依赖描述了一种类与类之间的使用与被使用的关系，如人过河依赖于船

**表示：**虚线+箭头，箭头指向被依赖的类

**体现：**被依赖的类在另一个类的方法中被调用

**类图：**

![依赖](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Dependency.png)

**代码：**
	
	public class Boat {
		public void crossRiver(){
			System.out.println("boat crossRiver");
    	}
    }
    
    public class Person {
    	//Boat在Person类的方法中被调用
    	public void crossRiver(){
       		Boat boat = new Boat();
       		boat.crossRiver();
    	}
	 }


##关联(Association)

**含义：**使一个类知道另一个类的属性和方法，如A关联B，则B体现为A的全局变量，如一般类和Util类

**表示：**实线+箭头，箭头指向被关联的类，可以为双向和单向
	
	双向:两个类都知道对方的公共属性和操作
	单向:一个类知道另一个类的公共属性和操作

**体现：**被关联的类是另一个类的属性

**类图：**

![关联](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Association.png)

**代码：**
	
	public class SomeClass{
		private SomeUtil util;
		...
	}

##聚合(Aggregation)

**含义：**关联关系的一种特例，体现的是整体拥有部分的关系,整体与部分之间可以分离，拥有各自的生命周期。部分可以属于多个整体。如汽车和轮胎、引擎的关系。汽车坏了，引擎和轮胎可以作为零件继续使用

**表示：**空心菱形+实线+箭头，箭头指向被聚合的类

**体现：**与关联关系一样，部分是整体的一个属性

**类图：**

![聚合](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Aggregation.png)

**代码：**
	
	public class Car{
		private Tyre tyre;
		private Engine engine;
		...
	}
	
##组合(Composition)

**含义：**也是关联关系的一种特例，是一种比聚合更强的关系，整体和部分具有相同的生命周期，当整体不在了，部分同时也就不在了。如人和手、脚的关系

**表示：**实心菱形+实线+箭头，箭头指向个体对象

**体现：**部分是整体的一个属性

**类图：**

![组合](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Composition.png)

**代码：**
	
	public class Person {
		private Hand hand;
    	private Foot foot;
    	...
	}

##泛化(Generalization)

**含义：**一般与特殊，一般与具体之间的关系，具体建立在特殊之上，并且进行了扩展。如动物和狗之间，狗除了拥有动物的属性之外还有自己特殊的属性

**表示：**空心三角形+实线,三角形的角指向父类

**体现：**子类继承父类

**类图：**

![泛化](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Generalization.png)

**代码：**

	public class Animal {
		...
	}
	public class Dog extends Animal{
		...
	}

##实现(Realization)

**含义：**类与接口之间的关系，类中是接口中定义方法的具体实现。如唐老鸭实现说话接口

**表示：**空心三角形+虚线，三角形的角指向接口

**体现：**类实现接口

**类图：**

![实现](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Realization.png)

**代码：**

	public interface SpeakIntf {
		public void speak();
	}
	public class Donald implements SpeakIntf {
		@Override
		public void speak() {
       		System.out.println("donald speak");
    	}
	}
	
##总结

`关联和聚合的区别`

主要在语义上，关联的两个对象之间一般是平等的，例如你是我的朋友，聚合则一般不是平等的，是整体和部分的关系

`聚合和组合的区别`

聚合关系是"has-a"关系，整体和部分可以拥有不同的生存周期。组合关系是"contains-a"关系，整体与部分的生存周期相同
	
