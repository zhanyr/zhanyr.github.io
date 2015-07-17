---
layout: post
category: DesignPattern
title: 设计模式之模板方法模式
tagline: by zhanyr
tags: [DesignPattern]
---

模板方法(Template Method)模式是基于继承的代码复用技术，广泛应用于框架设计中，以确保通过父类来控制处理流程的逻辑顺序（如框架的初始化，测试流程的设置等）

<!--more-->

###概述
	
模板方法模式定义一个操作中的算法的骨架，而将步骤延迟到子类中。模板方法使子类可以不改变整个算法的结构便可以重定义算法的某些特定步骤。

###类图

![模板方法类图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/TemplateMethod.png)

###代码实现

AbstractClass.java

	public abstract class AbstractClass {
		public abstract void method1();
       public abstract void method2();
       public void templateMethod(){
       		System.out.println("templateMethod");
        		method1();
        		method2();
    	}
	}

ConcreteClassA.java

	public class ConcreteClassA extends AbstractClass{
		@Override
		public void method1() {
			System.out.println("A_method1");
		}

		@Override
		public void method2() {
			System.out.println("A_method2");
		}
	}
	
ConcreteClassB.java

	public class ConcreteClassB extends AbstractClass {
		@Override
		public void method1() {
			System.out.println("B_method1");
		}

		@Override
		public void method2() {
			System.out.println("B_method2");
		}
	}
	
客户端代码

	public class TemplateMethodTest {
		@Test
		public void testTemplateMethod(){
			AbstractClass concreteA = new ConcreteClassA();
			AbstractClass concreteB = new ConcreteClassB();
			concreteA.templateMethod();
			concreteB.templateMethod();
		}
	}

###举个🌰

写论文，论文包括题目，摘要，引言，研究内容，总结，参考文献。每个人写论文都要包括这些部分，但每个人写的都不一样。这就是一种模板方法模式。设计一个论文抽象类，写具体的论文时，只要实现这个抽象类，重写这个抽象类里面的方法，就可以完成一篇论文。

论文抽象类:AbstractPaper.java

	public abstract class AbstractPaper {
    		public abstract void writeTitle();
    		public abstract void writeAbstract();
    		public abstract void writeIntroduction();
    		public abstract void writeResearch();
    		public abstract void writeSummary();
    		public abstract void writeReference();

    		public void writePaper(){
        		System.out.print("题目：");
        		writeTitle();
        		System.out.print("摘要：");
        		writeAbstract();
        		System.out.print("引言：");
        		writeIntroduction();
        		System.out.print("研究内容：");
        		writeResearch();
        		System.out.print("总结：");
        		writeSummary();
        		System.out.print("参考文献：");
        		writeReference();
    		}
	}
	
A同学写的论文:YangxyPaper.java

	public class YangxyPaper extends AbstractPaper {
    	@Override
    	public void writeTitle() {
        	System.out.println("论设计模式重要性");
    	}

    	@Override
    	public void writeAbstract() {
        	System.out.println("设计模式是一套被反复使用、多数人知晓的、" +
                "经过分类编目的、代码设计经验的总结");
    	}

    	@Override
    	public void writeIntroduction() {
        	System.out.println("很多框架都用到了设计模式");
    	}

    	@Override
    	public void writeResearch() {
        	System.out.println("设计模式很重要");
    	}

    	@Override
    	public void writeSummary() {
        	System.out.println("设计模式确实很重要");
    	}

    	@Override
    	public void writeReference() {
        	System.out.println("设计模式");
    	}
	}

B同学写的论文:ZhanyrPaper.java

	public class ZhanyrPaper extends AbstractPaper {
    	@Override
    	public void writeTitle() {
        	System.out.println("论吃的重要性");
    	}

    	@Override
    	public void writeAbstract() {
        	System.out.println("活着就是要吃");
    	}

    	@Override
    	public void writeIntroduction() {
        	System.out.println("不吃会饿啊");
    	}

    	@Override
    	public void writeResearch() {
        	System.out.println("吃是为了活着，活着不是为了吃");
    	}

    	@Override
    	public void writeSummary() {
        	System.out.println("吃确实很重要");
    	}

    	@Override
    	public void writeReference() {
        	System.out.println("十二道锋味");
    	}
	}
	
客户端代码：

	public class WriterPaperTest {
    	@Test
    	public void test(){
        	System.out.println("Yxy的paper:");
        	AbstractPaper yangxyPaper = new YangxyPaper();
        	yangxyPaper.writePaper();
        	System.out.println("Zhanyr的paper:");
        	AbstractPaper zhanyrPaper = new ZhanyrPaper();
        	zhanyrPaper.writePaper();
    	}
	}
	
结果:

	Yxy的paper:
	题目：论设计模式重要性
	摘要：设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结
	引言：很多框架都用到了设计模式
	研究内容：设计模式很重要
	总结：设计模式确实很重要
	参考文献：设计模式
	Zhanyr的paper:
	题目：论吃的重要性
	摘要：活着就是要吃
	引言：不吃会饿啊
	研究内容：吃是为了活着，活着不是为了吃
	总结：吃确实很重要
	参考文献：十二道锋味
	
###总结

**设计思想：**

作为模板的方法定义在父类（父类为抽象类），而方法定义使用抽象方法，实现抽象方法的是子类，要在子类实现方法，才能决定具体的操作。如果在不同的子类执行不同实现就可以发展出不同的处理内容。不过，无论在哪个子类执行任何一种实现，处理的大致流程都还是要依照父类制定的方式。

**优点：**

1. 模板方法模式把不变的行为搬到超类，去除了子类中的重复代码
2. 子类实现算法的某些细节，有助于算法的扩展
3. 通过子类扩展方法，符合开闭原则

**缺点：**

每个不同的实现都要定义一个子类，会导致类的个数过多

**适用场景：**

多个子类有公有的方法，并且逻辑相同

