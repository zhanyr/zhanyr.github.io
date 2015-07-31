---
layout: post
category: java
title: java深度学习之反射
tagline: by zhanyr
tags: [java, reflect]
---

Java反射机制容许程序在运行时加载、探知、使用编译期间完全未知的classes。换言之，Java可以加载一个运行时才得知名称的class，获得其完整结构。

<!--more-->

###概念
	
java反射式java被视为动态语言的一个关键性质。这个机制允许程序在运行时通过Reflection APIs取得任何一个已知名称的class的内部信息，包括其modifiers(如public,static等)、superclass(如Object)、实现之interface(例如Cloneable),也包括fields和methods的所有信息，并可于运行时改变fields内容或唤起methods.

简单来说，反射机制指的是程序在`运行时`能够获取自身的信息。在java中，只要给定类的名字，就可以通过反射机制来获取类的所有信息。

###反射机制的基础

java程序在运行时，java运行时系统一直对所有的对象进行所谓的运行时类型标识。折项信息记录了每个对象所属的类,虚拟机通常使用运行时类型信息选择正确的方法去执行，用来保存这些类型信息的类是Class类。也就是说：ClassLoader找到了需要调用的类时，就会加载它，然后根据.class文件内记载的信息来产生一个与该类相联系的独一无二的Class对象，该Class对象记载了该类的字段、方法等所有的信息。Class对象是jvm产生的。我们通过Class对象就可以在运行时获得类的所有信息，这就是反射技术的基础。

###使用场景

当只有一个类的定义，不能显示的创建一个类的对象的时候，可以用反射机制来进行操作。
如jdbc编程中，加载类驱动就是用反射机制来实现的：Class.forName("com.mysql.jdbc.Driver").newInstance();

###反射的使用方法

如果我们已知一个类的名字，那我们就可以通过使用反射机制获取类的所有相关信息。
首先根据类名获取Class对象：

	Class c = Class.forName("ClassName");//其中ClassName必须是全名，包括包名，类名等
	Object o = c.newInstance();//根据Class对象获得类的实例对象
	
获取Class对象还有另外两种方法：
	
	Class c = new ClassObject().getClass();
	Class c = ClassObject.class;
	
现在我们已经根据类名获取到了类的实例对象。接下来研究如何获取类的其他信息。

####获取类的属性

	//返回一个 Field 对象，它反映此 Class 对象所表示的类或接口的指定公共成员字段
	public Field getField(String name);
	//返回一个包含某些 Field 对象的数组，这些对象反映此 Class 对象所表示的类或接口的所有可访问公共字段
	public Field[] getFields();
	//返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段
	public FieldgetDeclaredField(Stringname);
	//返回 Field 对象的一个数组，这些对象反映此 Class 对象所表示的类或接口所声明的所有字段
	public Field[] getDeclaredFields();
	
**getFields和getDeclaredFields区别：**

**getFields返回的是申明为public的属性，包括父类中定义，
getDeclaredFields返回的是指定类定义的所有定义的属性，不包括父类的。**

####获取类的方法

	//返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法
	public Method getMethod(String name,Class<?>... parameterTypes);
	//返回一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法
	public Method[] getMethods();
	//返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法
	public MethodgetDeclaredMethod(Stringname,Class<?>... parameterTypes);
	//返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法
	public Method[] getDeclaredMethods();
	
####获取类的构造器

获取类的构造器的意义在于：调用构造器创建类的实例

	//返回一个 Constructor 对象，它反映此 Class 对象所表示的类的指定公共构造方法
	public Constructor<T> getConstructor(Class<?>... parameterTypes);
	//返回一个包含某些 Constructor 对象的数组，这些对象反映此 Class 对象所表示的类的所有公共构造方法
	public Constructor<?>[] getConstructors();
	//返回一个 Constructor 对象，该对象反映此 Class 对象所表示的类或接口的指定构造方法
	public Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes);
	//返回 Constructor 对象的一个数组，这些对象反映此 Class 对象表示的类声明的所有构造方法。它们是公共、保护、默认（包）访问和私有构造方法
	public Constructor<?>[] getDeclaredConstructors();
	
####新建类的实例

//调用类的默认构造器。如果没有默认构造器，则会调用失败
1. class.newInstance();	
2. Class c = Class.forName(ClassName);
	Constructor<?> constructor = c.getConstructor();
	Object inst = constructor.newInstance();
	
//调用带参数的构造器
	Constructor<?> constructor2 = c.getConstructor(int.class,String.class);
	Object inst2 = constructor2.newInstance(1,"zhanyr");

####调用类的函数

通过反射获取类的Method对象，调用Field的Invoke方法调用函数。

	Class c = Class.forName(ClassName);
	Object obj = c.newInstance();
	Method method = c.getDeclaredmethod(java.lang.String.class);
	method.setAccessible(true);
	method.invoke(obj,"test");
	
####获取类的属性值
	
	Class c = Class.forName(ClassName);
	Object obj = c.newInstance();
	Field field = c.getField("id");	
	field.setInt(obj,1);
	field.getInt(obj);
	
###反射的优缺点

####优点

反射机制的优点就是可以实现`动态`创建对象和编译，体现出很大的灵活性，特别是在J2EE的开发中它的灵活性就表现的十分明显。比如，一个大型的软件，不可能一次就把把它设计的很完美，当这个程序编译后，发布了，当发现需要更新某些功能时，我们不可能要用户把以前的卸载，再重新安装新的版本，假如这样的话，这个软件肯定是没有多少人用的。采用静态的话，需要把整个程序重新编译一次才可以实现功能 的更新，而采用反射机制的话，它就可以不用卸载，只需要在运行时才动态的创建和编译，就可以实现该功能。

####缺点

1. 性能问题：当用于字段和方法接入时反射要远慢于直接代码。性能问题的程度取决于程序中是如何使用反射的。如果它作为程序运行中相对很少涉及的部分，缓慢的性能将不会是一个问题。即使测试中最坏情况下的计时图显示的反射操作只耗用几微秒。仅反射在性能关键的应用的核心逻辑中使用时性能问题才变得至关重要。
2. 使用反射会模糊程序内部实际要发生的事情：程序人员希望在源代码中看到程序的逻辑，反射等绕过了源代码的技术会带来维护问题。反射代码比相应的直接代码更复杂，正如性能比较的代码实例中看到的一样。解决这些问题的最佳方案是保守地使用反射-- 仅在它可以真正增加灵活性的地方 -- 记录其在目标类中的使用。