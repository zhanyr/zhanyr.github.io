---
layout: post
category: DesignPattern
title: 设计模式之策略模式
tagline: by zhanyr
tags: [DesignPattern]
---

策略(Strategy)模式，又叫算法族模式，它定了一系列的算法，将这一系列中的每一个都对应的封装到一个类中，这些类实现同一个接口，就使得他们之间可以相互替换。这种模式让算法的变化不影响到客户端。

<!--more-->

###概述
	
策略模式是对算法的包装，是把使用算法的责任和算法本身分割开来，委派给不同的对象管理。策略模式通常把一系列的算法包装到一系列的策略类中，作为一个抽象策略类的子类。用一句话说就是：准备一个算法策略接口，再准备一组具体的算法类分别实现这个接口；创建一个环境角色类持有这个策略接口的引用。当某个客户端要使用算法时，创建对应算法的实例传入环境角色类即可。

###类图

![策略模式类图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/strategy.png)

###代码实现

策略方法接口：Strategy.java

	public interface Strategy {
		public void algorithmMethod();
	}

具体算法：ConcreteStrategyA.java

	public class ConcreteStrategyA implements Strategy{
		@Override
		public void algorithmMethod() {
			System.out.println("ConcreteStrategyA");
		}
	}
	
ConcreteStrategyB.java
	
	public class ConcreteStrategyB implements Strategy{
		@Override
		public void algorithmMethod() {
			System.out.println("ConcreteStrategyB");
		}
	}

场景：Context.java

	public class Context {
		private Strategy strategy;
		//默认构造方法
		public Context(){}
		//使用strategy构造
		public Context(Strategy strategy){
			this.strategy = strategy;
		}
		//策略方法
		public void strategyMethod(){
			strategy.algorithmMethod();
		}
	}

客户端：StrategyTest.java

	public class StrategyTest {
		@Test
		public void testStrategy(){
			System.out.print("使用A算法:");
			Strategy strategyA = new ConcreteStrategyA();
			Context contextA = new Context(strategyA);
			contextA.strategyMethod();

			System.out.print("使用B算法:");
			Strategy strategyB = new ConcreteStrategyB();
			Context contextB= new Context(strategyB);
			contextB.strategyMethod();
		}
	}
	
###举个🌰

为庆祝折800公司上市，公司领导决定组织一次超级大促活动。将用户按积分分成三个等级：初级会员，中级会员，高级会员。初级会员可以享受满100减20的优惠；中级会员享受8折优惠；高级会员享受满200减20之后7折优惠,如果不满200则享受8折优惠。

促销优惠策略接口：PromotionStategy.java

	public interface PromotionStategy {
		public double calcDiscountPrice(double totalPrice);
	}
	
初级会员优惠策略：PrimaryMemberStrategy.java

	public class PrimaryMemberStrategy implements PromotionStategy {
		//初级会员满100-20
		@Override
		public double calcDiscountPrice(double totalPrice) {
			if(totalPrice >= 100)
				return totalPrice-20;
			return totalPrice;
		}
	}

中级会员优惠策略：IntermediateMemberStrategy.java

	public class IntermediateMemberStrategy implements PromotionStategy {
		//中级会员8折
		@Override
		public double calcDiscountPrice(double totalPrice) {
			return totalPrice*0.8;
		}
	}
	
高级会员优惠策略：

	public class SeniorMemberStrategy implements PromotionStategy{
		//高级会员满200减20之后7折优惠,如果不满200则享受8折优惠
		@Override
		public double calcDiscountPrice(double totalPrice) {
			if (totalPrice >= 200){
				return (totalPrice-20)*0.7;
			}
			return totalPrice*0.8;
		}
	}
	
计算应付总价：

	public class CalcDiscountPrice {
		private PromotionStategy promotionStategy;
		public CalcDiscountPrice(){
		}
		public  CalcDiscountPrice(PromotionStategy promotionStategy){
			this.promotionStategy = promotionStategy;
		}
		public double CalcTotal(double price){
			return promotionStategy.calcDiscountPrice(price);
		}
	}
	
客户端代码：

	public class PromotionStrategyTest {
		@Test
		public void testPromotionStrategy(){
			System.out.print("某初级用户花了150，总价是：");
			PromotionStategy stategyA = new PrimaryMemberStrategy();
			CalcDiscountPrice calcDiscountPriceA = new CalcDiscountPrice(stategyA);
			System.out.println(calcDiscountPriceA.CalcTotal(150));

			System.out.print("某中级用户花了80，总价是：");
			PromotionStategy stategyB = new IntermediateMemberStrategy();
			CalcDiscountPrice calcDiscountPriceB = new CalcDiscountPrice(stategyB);
			System.out.println(calcDiscountPriceB.CalcTotal(80));

			System.out.print("某高级用户花了220，总价是：");
			PromotionStategy stategyC = new SeniorMemberStrategy();
			CalcDiscountPrice calcDiscountPriceC = new CalcDiscountPrice(stategyC);
			System.out.println(calcDiscountPriceC.CalcTotal(220));
		}
	}
	
结果：

	某初级用户花了150，总价是：130.0
	某中级用户花了80，总价是：64.0
	某高级用户花了220，总价是：140.0
	
###总结

**设计思想：**

把一个类中经常改变或者将来可能改变的部分提取出来，作为一个接口，然后在一个专用的类中包含这个对象的实例，这样类的实例在运行时就可以随意调用实现了这个接口的类的行为。

**优点：**

1. 易于扩展，增加一个新的策略非常容易，在不改变源代码的基础上创建新的策略类即可。
2. 如果不使用策略模式，对于所有的算法，必须使用条件语句进行连接，通过条件判断来决定使用哪一种算法，多重条件语句不易维护。使用策略模式可以避免使用多重条件(if-else)语句。

**缺点：**

1. 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道算法或行为的情况。
2. 由于策略模式把每个具体的策略实现都单独封装成为类，如果策略很多的话，那么类的数量就会很多，很难维护。

**适用场景：**

1. 几个类的主要逻辑相同，只在部分逻辑的算法和行为上稍有区别。
2. 有几种相似的策略或算法，客户端需要动态地决定使用哪一种，就可以使用策略模式，将这些策略或算法封装起来供客户端调用。