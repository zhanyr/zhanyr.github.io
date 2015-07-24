---
layout: post
category: DesignPattern
title: 设计模式之命令模式
tagline: by zhanyr
tags: [DesignPattern]
---

命令模式将请求封装成对象，把命令调用与命令执行区分开来，是一种分离耦合、提高重用的方法。它的应用非常广泛，本文将详细介绍命令模式及其使用方法。

<!--more-->

###概述
	
我们经常会遇到这样的一些情况：设计界面时，同样的菜单控件在不同的应用环境中的功能是完全不同的；电子产品的按钮在不同的情况下作用不同。就手机来说，按照最差最原始的设计，每个功能都要对应一个按钮或菜单，这完全是不可行的。此时，运用分离变化与不变的因素，将菜单或按钮触发的功能分离开来，只提供一个统一的触发接口。这样就提高了很大的重用性。

《设计模式》中将命令模式定义为：将一个请求封装成一个对象，从而可以用不同的请求对客户进行参数化，对请求排队或记录请求日志，支持可撤销的操作。

###类图

![命令模式类图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/Command.png)

其中：

Command接口:定义了相关操作

ConcreteConmand:接口的具体实现，持有一个Reciver命令，并通过调用Receiver的action方法执行具体的操作

Reciver:请求的最终执行者

Invoker:负责接收和执行命令，也可以对命令排队，执行命令撤销等

###代码实现

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

###举个🌰


	
###总结

**设计思想：**



**优点：**



**缺点：**



**适用场景：**

