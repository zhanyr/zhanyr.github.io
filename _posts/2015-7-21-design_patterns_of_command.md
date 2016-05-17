---
layout: post
category: DesignPattern
title: 设计模式之命令模式
tagline: by zhanyr
tags: [DesignPattern]
---

命令模式将请求封装成对象，把命令调用与命令执行区分开来，是一种分离耦合、提高重用的方法。它的应用非常广泛，本文将详细介绍命令模式及其使用方法。

<!--more-->

### 概述
	
我们经常会遇到这样的一些情况：设计界面时，同样的菜单控件在不同的应用环境中的功能是完全不同的；电子产品的按钮在不同的情况下作用不同。就手机来说，按照最差最原始的设计，每个功能都要对应一个按钮或菜单，这完全是不可行的。此时，运用分离变化与不变的因素，将菜单或按钮触发的功能分离开来，只提供一个统一的触发接口。这样就提高了很大的重用性。

《设计模式》中将命令模式定义为：将一个请求封装成一个对象，从而可以用不同的请求对客户进行参数化，对请求排队或记录请求日志，支持可撤销的操作。

### 类图

![命令模式类图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Command.png)

其中：

Command接口:定义了相关操作

ConcreteConmand:接口的具体实现，持有一个Reciver命令，并通过调用Receiver的action方法执行具体的操作

Reciver:请求的最终执行者

Invoker:负责接收和执行命令，也可以对命令排队，执行命令撤销等

### 代码实现

Command.java

	public interface Command {
		public void execute();
	}

ConcreteCommand1.java
	
	public class ConcreteCommand1 implements Command {
		private Reciever reciever;
		public ConcreteCommand1(Reciever reciever) {
			this.reciever = reciever;
		}
		@Override
		public void execute() {
			System.out.println("this is Command1");
			reciever.action();
		}
	}
	
ConcreteCommand2.java

	public class ConcreteCommand2 implements Command {
		private Reciever reciever;
		public ConcreteCommand2(Reciever reciever) {
			this.reciever = reciever;
		}
		@Override
		public void execute() {
			System.out.println("this is Command2");
			reciever.action();
		}
	}
	
Receiver.java

	public class Reciever {
		String name = "";
		public Reciever(String name){
			this.name = name;
		}
		public void action(){
			System.out.println("Reciever "+name+" action");
		}
	}
	
Invoker.java
	
	public class Invoker {
		private Command command;
		public void setCommand(Command command){
			this.command = command;
		}
		public void executeCommand(){
			this.command.execute();
		}
	}
	
CommandClient.java
	
	public class CommandClient {
		@Test
		public void testCommand(){
			Command command1 = new ConcreteCommand1(new Reciever("zhanyr"));
			Invoker invoker1 = new Invoker();
			invoker1.setCommand(command1);
			invoker1.executeCommand();

			Command command2 = new ConcreteCommand2(new Reciever("zhanyaru"));
			Invoker invoker2 = new Invoker();
			invoker2.setCommand(command2);
			invoker2.executeCommand();
		}
	}
	
结果:

this is Command1
Reciever zhanyr action
this is Command2
Reciever zhanyaru action

### 举个🌰

遥控器开关
	
### 总结

**设计思想：**

 Command命令模式是一种对象行为型模式，它主要解决的问题是：在软件构建过程中，"行为请求者"与"行为实现者"通常呈现一种"紧耦合"的问题

**优点：**

1. 封装性好：每个命令都被封装起来，对于客户端来说，需要什么功能就去调用相应的命令，而无需知道命令具体是怎么执行的。
2. 扩展性好：在命令模式中，在接收者类中一般会对操作进行最基本的封装，命令类则通过对这些基本的操作进行二次封装，当增加新命令的时候，对命令类的编写一般不是从零开始的，有大量的接收者类可供调用，也有大量的命令类可供调用，代码的复用性很好。

**缺点：**

如果命令很多的话，具体的实现类也会随着增加，难以管理。特别是很多简单的命令，实现起来就几行代码的事，而使用命令模式的话，不用管命令多简单，都需要写一个命令类来封装。

**适用场景：**

1. 整个调用过程比较繁杂，或者存在多处这种调用。这时，使用Command类对该调用加以封装，便于功能的再利用。
2. 调用前后需要对调用参数进行某些处理。
3. 调用前后需要进行某些额外处理，比如日志，缓存，记录历史操作等。

