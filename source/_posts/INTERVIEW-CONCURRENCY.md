---
title: INTERVIEW_CONCURRENCY
comments: true
date: 2018-07-02 16:15:24
tags: inter
categories: 
---



#####  线程基础

###### ? 进程和线程的区别 协程呢

* 进程  

​	是系统给程序分配资源的基本单位，每个进程都有唯一的地址空间，一个程序至少有一个进程，一个进程至少有一个线程.

* 线程

  线程是执行操作的基本单位，JVM结构中共享 Method Area ,Heap Area

​	https://blog.csdn.net/mxsgoden/article/details/8821936

​	https://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html

进程 : 有很大的独立性

线程 :  所有线程都有完全一样的地址空间,意味着它们也共享同样的全局变量。由于线程可以访问进程地址空间的每一个内存地址，所以一个线程可以读、写甚至清除另一个线程的堆栈。

每个进程中的内容 : 地址空间  全局变量 打开文件 子进程	即将发生的定时器	信号与信号处理程序

每个线程中的内容：程序计数器、寄存器、堆栈、状态.



协程?

​		协程拥有自己的寄存器上下文和栈，协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的	时候，恢复先前保存的寄存器上下文和栈，即所有局部状态的一个特定组合），每次过程重入时，就相当于进入上一次调用的状态，换种说法：进入上一次离开时所处逻辑流的位置。

协程的好处:

1. 无需线程上下文切换的开销 
2. 无需原子操作锁定及同步的开销
3. 方便切换控制流，简化编程模型

​	为什么要有线程，而不是仅仅用进程？



###### 怎么创建一个线程

1. 继承Thread类创建线程类.

2. 通过Runnable接口创建线程类

3. 通过Callable和FutureTask创建线程

   ```java
   public class CallableThreadTest implements Callable<Integer> {
   
       public static void main(String[] args) {
           CallableThreadTest ctt = new CallableThreadTest();
           FutureTask<Integer> ft = new FutureTask<>(ctt);
           for (int i = 0; i < 100; i++) {
               System.out.println(Thread.currentThread().getName() + " 的循环变量i的值" + i);
               if (i == 20) {
                   new Thread(ft, "有返回值的线程").start();
               }
           }
           try {
               System.out.println("子线程的返回值：" + ft.get());
           } catch (InterruptedException e) {
               e.printStackTrace();
           } catch (ExecutionException e) {
               e.printStackTrace();
           }
       }
   
       @Override
       public Integer call() throws Exception {
           int i = 0;
           for (; i < 100; i++) {
               System.out.println(Thread.currentThread().getName() + " " + i);
           }
           return i;
       }
   }
   ```

###### 线程如何关闭？

​	最正确的停止线程的方式是使用 interrupt,和条件满足.

```java
while (!Thread.currentThread().isInterrupted() && cout<1000) {
    do more work
}
```

###### 在Java中wait和sleep方法的不同

	1. sleep作用Thread上，wait作用object上
	2. sleep不会释放锁
	3. sleep可以在任何代码块。wait必须在同步方法，持有锁中运行。

​	

谈谈wait/notify关键字的理解

1. 调用之前持有对象锁
2. wait ,线程进入 waiting状态，释放对象锁
3. notify,唤醒处于waiting状态的线程。

https://howtodoinjava.com/java/multi-threading/wait-notify-and-notifyall-methods/





###### 如何控制某个方法允许并发访问线程的个数？

​	信号量

###### 什么导致线程阻塞？

1.  阻塞指的是暂停一个线程的执行以等待某个条件发生。 Thread.sleep t.join 等待输入
2. 线程执行了一个对象的`wait()`方法，直接进入阻塞状态，等待其他线程执行`notify()`或者`notifyAll()`方法。

###### 如何保证线程安全？

	1. 使用线程安全的类。
	2. 使用synchronized同步代码块，或者用Lock锁。

###### 如何实现线程同步？

 	1. Synchronized修饰整个方法或代码块。
 	2. **Lock**

###### 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？

​	可以进行同时读写，但为了保证数据的正确，必须要针对进程访问的共享临界区进行处理；两个进程不能同时进入临界区，否则会导致数据错乱。常见的处理方式有：**信号量、管程、会合、分布式系统**

* **信号量**
   信号量是一个计数器，它只支持2种操作：P操作（进入临界区）和V操作（退出临界区）。假设有信号量SV，则对它的P、V操作含义如下：
     P(SV)，如果SV的值大于0，意味着可以进入临界区，就将它减1；如果SV的值为0，意味着别的进程正在访问临界区，则挂起当前进程的执行；
     V(SV)，当前进程退出临界区时，如果有其他进程因为等待SV而挂起，则唤醒之；如果没有，则将SV加1，之后再退出临界区。

*  **管程**
     提出原因：信号量机制不足，程序编写困难、易出错
         方案：在程序设计语言中引入一种高级维护机制
         定义：是一个特殊的模块；有一个名字；由关于共享资源的数据结构及在其上操作上的一组过程组成。进程只能通过调用管程中的过程间接访问管程中的数据结构
    1）互斥：管程是互斥进入的 为了保证数据结构的数据完整性
   管程的互斥由编译器负责保证的，是一种语言机制
    2）同步：设置条件变量及等待唤醒操作以解决同步问题
   可以让一个进程或者线程在条件变量上等待（先释放管程的管理权），也可以通过发送信号将等待在条件变量上的进程线程唤醒

  链接：https://www.jianshu.com/p/72f5017c6649

###### 线程间操作List

​	

#### 锁

##### 线程安全 锁

###### 讲一下java中的同步的方法 结果锁一起



synchronize的原理

- 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
- static synchronized 方法的多线程访问和作用
- 同一个类里面两个synchronized方法，两个线程同时访问的问题
- synchronized与Lock的区别
- ReentrantLock 、synchronized和volatile比较
- ReentrantLock的内部实现
- lock原理
- 死锁的四个必要条件？
- 怎么避免死锁？
- 对象锁和类锁是否会互相影响？
- synchronized是公平锁还是非公平锁,ReteranLock是公平锁吗？是怎么实现的
- synchronized跟ReentranLock有什么区别？
- synchronized与ReentranLock发生异常的场景.



##### 管理线程 提高效率

- volatile的原理

- 谈谈volatile关键字的用法 谈谈volatile关键字的作用

- 谈谈NIO的理解

- synchronized 和volatile 关键字的区别

- 什么是线程池，如何使用?

- Java的并发、多线程、线程模型

- 谈谈对多线程的理解

- 多线程有什么要注意的问题？

- 谈谈你对并发编程的理解并举例说明

- 谈谈你对多线程同步机制的理解？

- 如何保证多线程读写文件的安全？

- 多线程断点续传原理

- 断点续传的实现

- 为什么要用线程池

  

##### 线程池

- JavaAPI线程池有哪些参数

- 什么是核心线程

- 怎么销毁核心线程

- 为什么DCL要那么写，直接在方法前加synchronized不行吗

- 源码中有哪里用到了AtomicInt

- 如何让两个线程循环交替打印

- 怎么中止一个线程，Thread.Interupt一定有效吗？

- 协程可以在Java项目中使用吗？

- 线程池了解多少？拒绝策略有几种,为什么有newSingleThread

- 跨进程通信了解多少？管道了解吗？

- 线程中 sleep() 和 wait() 有何区别，各有什么含义？

- 安卓解决线程并发问题。

  

##### 底层原理

- RXJava怎么切换线程
- binder进程间通信可以调用原进程方法吗？
- SharedPreference原理？读取xml是在哪个线程?
- AQS了解吗？
- ConcurrentHashMap，线程安全，为何安全。底层实现是怎么样的。

