---
layout: post
category: multithreading
title: java中的多线程(二)线程安全
tagline: by Zhanyr
tags: [multithread, java]
---
当多个线程同时读写同一份共享资源的时候，可能会引起冲突。这时候，我们需要引入线程"同步"机制，即各位线程之间要有个先来后到，不能一窝蜂挤上去抢作一团。

<!--more-->

### 同步的前提

* 必须有两个或两个以上的线程

* 必须是多个线程使用同一个锁

### 同步的优缺点

* 优点：解决了多线程的安全问题 

* 缺点：多个线程需要判断锁，比较消耗资源

### 同步函数

同步有两种表现形式：同步代码块，同步函数

* 同步代码块：
	
		synchronized(obj){
		//同步代码块
		}

* 同步函数：

		public synchronized void method(){
		
		}
		
* 静态同步函数
      
		public static synchronized void method(){
		
		}
		
同步代码块使用的锁是括号中括的obj,同步函数使用的锁是this,静态同步函数使用的锁不再是this,因为静态方法中不可以定义this,静态同步函数的锁是Class对象。（静态进内存时，内存中没有本类对象，但是一定有该类对应的字节码文件对象`本类.class`）

### 写一个线程安全的单例模式

#### 饿汉式

	public class ESingle {
		private  static final ESingle s = new ESingle();
		private ESingle(){}
		public static ESingle getESingle(){
			return s;
		}
	}

不存在线程安全问题

#### 懒汉式

延迟加载

	public class LSingle {
		private LSingle(){}
		private static LSingle single = null;
		public LSingle getInstance(){
			if(null == single)
				single = new LSingle();
			return single;
		}
	}

存在线程安全问题，当多线程同时访问getInstance方法时，可能会导致同时new LSingle()

改成线程安全的形式如下:

	public class LSingle {
		private LSingle(){}
		private static LSingle single = null;
		public LSingle getInstance(){
			if(null != single){
				synchronized (LSingle.class) {
					if(null == single)
						single = new LSingle();
				}
			}
			return single;
		}
	}

### 死锁

同步中嵌套同步会造成死锁,线程1持有锁A,想获取锁B,线程2持有锁B,想获取锁A。这个时候会造成死锁。

### 写一个死锁的例子

	public class DeadLock implements Runnable {
		private boolean flag;
		private Object objecta = new Object();
		private Object objectb = new Object();

		public DeadLock(boolean flag, Object obj1, Object obj2) {
			this.flag = flag;
			this.objecta = obj1;
			this.objectb = obj2;
		}

		@Override
		public void run() {
			if (flag) {
				synchronized (objecta) {
					System.out.println("if have objecta");
					synchronized (objectb) {
						System.out.println("if have objectb");
					}
				}
			} else {
				synchronized (objectb) {
					System.out.println("else have objectb");
					synchronized (objecta) {
						System.out.println("else have objecta");
					}
				}
			}
		}

		public static void main(String[] args) {
			Object objecta = new Object();
				Object objectb = new Object();
			Thread t1 = new Thread(new DeadLock(true, objecta, objectb));
			Thread t2 = new Thread(new DeadLock(false, objecta, objectb));
			t1.start();
			t2.start();
		}
	}