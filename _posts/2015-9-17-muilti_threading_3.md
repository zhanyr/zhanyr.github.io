---
layout: post
category: multithreading
title: java中的多线程(三)线程间通信及线程优先级
tagline: by Zhanyr
tags: [multithread, java]
---
线程间通信，实际上就是多个线程操作同一个资源，但是操作的动作不同。比较典型的例子就是生产者消费者问题。

<!--more-->

###等待唤醒机制

wait(),notify(),notifyAll()都使用在同步中，因为要对持有监视器（锁）的线程操作，只有同步才具有锁的概念。所以要使用在同步中。

为什么这些操作线程的方法要定义在Object类中呢？
因为这些方法在操作同步线程时，都必须要标识它们所操作线程的锁，`只有同一个锁上的被等待线程，可以被同一个锁上的notify唤醒`。也就是说，等待和唤醒必须是同一个锁。由于锁可以是任意对象，意味着这些方法可能被任意对象调用，所以要定义在Object中。

###写一个生产者消费者例子

[ProducerConsumer](https://github.com/zhanyr/Programming-questions/blob/master/ProducerConsumer.java)


###线程停止

stop方法过时，停止线程只有一种方法：run方法结束。

实际应用中，多线程运行代码通常是循环结构，只要控制住循环，让run方法结束，就可以让线程结束。

特殊情况：当线程处于冻结状态，就无法读取到标记，那么线程就不会结束。


###线程中断

当没有指定的方式让冻结的线程恢复到冻结状态时，需要对冻结状态进行清除，强制让线程恢复到运行状态中来，这样就可以操作标记，让线程结束。Thread类中提供了Interrupt方法。

###守护线程
setDaemon(boolean);

Daemon的作用是为其他线程的运行提供服务，比如说GC线程。其实User Thread线程和Daemon Thread守护线程本质上来说去没啥区别的，唯一的区别之处就在虚拟机的离开：如果User Thread全部撤离，那么Daemon Thread也就没啥线程好服务的了，所以虚拟机也就退出了。

###join方法

申请cpu执行权。(`临时加入线程`)主线程遇到join()方法，就将执行权释放，具体谁抢到执行权由CPU决定。

在很多情况下，主线程生成并起动了子线程，如果子线程里要进行大量的耗时的运算，主线程往往将于子线程之前结束，但是如果主线程处理完其他的事务后，需要用到子线程的处理结果，也就是主线程需要等待子线程执行完成之后再结束，这个时候就要用到join()方法了。

主线程等待join的子线程挂了的时候才开始继续运行。

###线程优先级

Java线程的优先级是一个整数，其取值范围是1 （Thread.MIN_PRIORITY ） - 10 （Thread.MAX_PRIORITY ）。主线程默认值是5(Thread.NORM_PRIORITY)。子线程默认的优先级是父线程的优先级。可以通过setPriority方法（final的，不能被子类重载）更改优先级。

###yield方法

Thread.yield()方法作用是：暂停当前正在执行的线程对象，并执行其他线程。`yield()应该做的是让当前运行线程回到可运行状态`，以允许具有相同优先级的其他线程获得运行机会。因此，使用yield()的目的是让相同优先级的线程之间能适当的轮转执行。但是，实际中无法保证yield()达到让步目的，因为让步的线程还有可能被线程调度程序再次选中。

###join,yield，sleep等方法的区别

参考 [java之yield(),sleep(),wait()区别详解-备忘笔记](http://dylanxu.iteye.com/blog/1322066)

