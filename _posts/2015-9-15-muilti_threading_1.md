---
layout: post
category: multithreading
title: java中的多线程(一)基本概念
tagline: by Zhanyr
tags: [multithread, java]
---
`进程`是计算机中正在运行的程序。一般情况下，在某一时刻，CPU只能执行一个进程。`线程`是一个进程中不同的执行路径。进程中必然存在至少一个线程。

<!--more-->

###进程与线程

#####进程

进程是一个正在执行中的程序，每个进程执行都有一个执行顺序，该顺序是一个执行路径，或者叫一个控制单元。

#####线程

线程就是进程中一个独立的控制单元，线程在控制着进程的执行。一个进程中至少有一个线程。

```
jvm启动的时候会产生一个进程java.exe.该进程中至少有一个线程负责java程序的执行，而且这个线程运行的代码存在于main方法中，称之为`主线程`。更加详细的说:jvm启动时不只是只有一个线程，还有回收垃圾的线程。
```

###创建线程

两种方式：

方式一：继承Thread类,覆盖run方法，通过start方法开启线程


	package com.zhanyr.muiltyThread;

	public class ThreadDemo extends Thread{
		public void run(){
			for(int i = 0;i<100;i++){
				System.out.println("thread run---"+i);
			}
		}
	}



	package com.zhanyr.muiltyThread;

	public class Test {
		public static void main(String[] args) {
			ThreadDemo threadDemo = new ThreadDemo();
			threadDemo.start();//开启线程并执行线程的run方法
			for(int i=0;i<100;i++){
				System.out.println("main----"+i);
			}
		}
	}


方式二：实现Runnable接口：
定义类实现Runnable接口，覆盖接口中的run方法，通过Thread类建立线程对象，将Runnable接口的子类对象作为实际参数传递给Thread类的构造函数，调用Thread类的start方法开启线程并调用Runnable接口的子类的run方法。

	package com.zhanyr.muiltyThread.implRunnable;

	public class Demo1 implements Runnable {

		@Override
		public void run() {
			for(int i =0;i<100;i++){
				System.out.println(Thread.currentThread().getName()+"run----"+i);
			}

		}
	
		public static void main(String[] args) {
			Demo1 demo1 = new Demo1();
			Thread t1 = new Thread(demo1);
			Thread t2 = new Thread(demo1);
			t1.start();
			t2.start();
		}
	}



Thread t = new Thread(SomeClass);

//之所以将实现Runnable接口的子类对象传递给Thread的构造函数，是因为，自定义的run方法属于定义的类，要用start启动线程，让线程运行run方法，就必须明确该run方法所属对象




###线程状态

![线程状态图](https://github.com/zhanyr/zhanyr.github.io/raw/master/_images/threadStatus.png)

###线程安全问题

多个线程同时操作一个变量，会产生莫名其妙的错误结果。

问题的原因在于：当多条语句在操作同一个线程共享数据时，一个线程对多条语句只执行了一部分，还没有执行完，另一个线程参与进来执行，导致了共享数据的错误。

问题解决：对多条操作共享数据的语句作为一个语句块，一个线程将自己所属的语句块执行完，另一个线程在执行，就能够解决此问题。

java中的解决方式：`同步代码块`

    synchronized(对象){
    	需要被同步的代码
	}

`对象相当于锁` 持有锁的线程可以在同步中执行，没有持有锁的线程即使获得了cpu得执行权，也进不去，因为没有获取锁
