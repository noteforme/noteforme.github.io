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

###### 在Java中wait和sleep方法的不同；



- 谈谈wait/notify关键字的理解

- 讲一下java中的同步的方法

- 如何控制某个方法允许并发访问线程的个数？

  

##### 线程安全 锁

- 什么导致线程阻塞？
- 数据一致性如何保证？
- 如何保证线程安全？
- 如何实现线程同步？
- 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
- 线程间操作List
- Java中对象的生命周期
- Synchronized用法
- synchronize的原理
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

