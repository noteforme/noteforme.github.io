---
title: INTERVIEW_CONCURRENCY
comments: true
date: 2018-07-02 16:15:24
tags: inter
categories: 
---



https://noteforme.github.io/tags/concurrency/



###  线程基础

##### Java线程模型

1. 用户线程与内核级线程
2. 并发与并行
3. 多线程模型

https://crazyfzw.github.io/2018/06/19/thred-model/

谈谈对多线程的理解

1. 在Android中一个应用程序就是一个单独的进程，一般来说，当我们运行一个应用，系统就会自动创建一个进程，并且为这个进程创建一个主线程--UI线程，这样就可以运行MainActivity。
2. 线程是操作系统能够进行运算调度的最小单位，线程是进程的子集，线程可以并行的执行不同任务，所有的线程共享同一片内存空间，这就为线程间通信提供了基础，线程有五种状态：创建，就绪，运行，阻塞，死亡。

##### 多线程有什么要注意的问题？

并发问题，安全问题，效率问题。

谈谈你对并发编程的理解并举例说明

谈谈你对多线程同步机制的理解？

##### ? 进程和线程的区别 协程呢

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



##### 怎么创建一个线程

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

##### 线程如何关闭？

​	最正确的停止线程的方式是使用 interrupt,和条件满足.

```java
while (!Thread.currentThread().isInterrupted() && cout<1000) {
    do more work
}
```

##### 在Java中wait和sleep方法的不同

	1. sleep作用Thread上，wait作用object上
	2. sleep不会释放锁
	3. sleep可以在任何代码块。wait必须在同步方法，持有锁中运行。

​	

谈谈wait/notify关键字的理解

1. 调用之前持有对象锁
2. wait ,线程进入 waiting状态，释放对象锁
3. notify,唤醒处于waiting状态的线程。

https://howtodoinjava.com/java/multi-threading/wait-notify-and-notifyall-methods/



##### 如何控制某个方法允许并发访问线程的个数？

​	信号量

##### 什么导致线程阻塞？

1.  阻塞指的是暂停一个线程的执行以等待某个条件发生。 Thread.sleep t.join 等待输入
2. 线程执行了一个对象的`wait()`方法，直接进入阻塞状态，等待其他线程执行`notify()`或者`notifyAll()`方法。

##### 如何保证线程安全？

	1. 使用线程安全的类。
	2. 使用synchronized同步代码块，或者用Lock锁。

##### 如何实现线程同步？

 	1. Synchronized修饰整个方法或代码块。
 	2. Lock

##### 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？

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

##### 线程间操作List

​	

##### 怎么中止一个线程，Thread.Interupt一定有效吗？

不一定 

https://juejin.cn/post/6844903896339447815#heading-6



### 线程安全 锁

##### 讲一下java中的同步的方法 结果锁一起

 1. synchronized  

 2. wait和notify

 3. volatile

      a. volatile关键字为域变量的访问提供了一种免锁机制
      b.使用volatile修饰域相当于告诉虚拟机该域可能会被其他线程更新
      c.因此每次使用该域就要重新计算，而不是使用寄存器中的值 
      d.volatile不会提供任何原子操作，它也不能用来修饰final类型的变量 

##### synchronize的原理

> 1. 原子性：确保线程互斥的访问同步代码；
> 2. 可见性：保证共享变量的修改能够及时可见，其实是通过Java内存模型中的 “**对一个变量unlock操作之前，必须要同步到主内存中；如果对一个变量进行lock操作，则将会清空工作内存中此变量的值，在执行引擎使用此变量前，需要重新从主内存中load操作或assign操作初始化变量值**” 来保证的；
> 3. 有序性：有效解决重排序问题，即 “一个unlock操作先行发生(happen-before)于后面对同一个锁的lock操作”；

##### 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解

1. 当synchronized作用于普通方法是，锁对象是this；
2. 当synchronized作用于静态方法是，锁对象是当前类的Class对象；
3. 当synchronized作用于代码块时，锁对象是synchronized(obj)中的这个obj。

##### static synchronized 方法的多线程访问和作用

也就是两个的区别了,也就是synchronized相当于this.synchronized，而static synchronized相当于Something.synchronized，它可以对类的所有对象实例起作用

##### synchronized与Lock的区别

Lock支持的功能:

公平锁：Synchronized是非公平锁，ReentrantLock支持公平锁，默认非公平锁

可中断锁：ReentrantLock提供了lockInterruptibly（）的功能，可以中断争夺锁的操作，抢锁的时候会check是否被中断，中断直接抛出异常，退出抢锁。而Synchronized只有抢锁的过程，不可干预，直到抢到锁以后，才可以编码控制锁的释放。

快速反馈锁：ReentrantLock提供了trylock（）  和 trylock（tryTimes）的功能，不等待或者限定时间等待获取锁，更灵活。可以避免死锁的发生。

读写锁：ReentrantReadWriteLock类实现了读写锁的功能，类似于Mysql，锁自身维护一个计数器，读锁可以并发的获取，写锁只能独占。而synchronized全是独占锁

Condition：ReentrantLock提供了比Sync更精准的线程调度工具，Condition，一个lock可以有多个Condition，比如在生产消费的业务下，一个锁通过控制生产Condition和消费Condition精准控制。

https://www.jianshu.com/p/09d5ba4bfb7a


##### ReentrantLock的内部实现

显式锁ReentrantLock和同步工具类的实现基础都是AQS (AbstractQueuedSynchronizer).AQS内部有一条双向的队列存放等待线程，节点是Node对象。每个Node维护了线程、前后Node的指针和等待状态等参数。



ReentrantLock是可重入锁，也就是同一个线程可以多次获取锁，每获取一次就会进行一次计数，解锁的时候就会递减这个计数，直到计数变为0。

它有两种实现，一种是公平锁，一种是非公平锁，

##### lock原理

整体来看Lock主要是通过两个东西来实现的分别是**CAS和AQS(AbstractQueuedSynchronizer)。**通过加锁和解锁的过程来分析锁的实现。

一、整体概述流程

1. 读取表示锁状态的变量

2. 如果表示状态的变量的值为0，那么当前线程尝试将变量值设置为1（通过CAS操作完成），当多个线程同时将表示状态的变量值由0设置成1时，仅一个线程能成功，其它线程都会失败。失败后进入队列自旋转并阻塞当前线程。

     2.1 若成功，表示获取了锁，

     > 2.1.1 如果该线程（或者说节点）已位于在队列中，则将其出列（并将下一个节点则变成了队列的头节点）
     
     > 2.1.2 如果该线程未入列，则不用对队列进行维护
     
     > 2.1.3 然后当前线程从lock方法中返回，对共享资源进行访问。           
     
      2.2 若失败，则当前线程将自身放入等待（锁的）队列中并阻塞自身，此时线程一直被阻塞在lock方法中，没有从该方法中返回（被唤醒后仍然在lock方法中，并从下一条语句继续执行，这里又会回到第1步重新开始）。
     
3. 如果表示状态的变量的值为1，那么将当前线程放入等待队列中，然后将自身阻塞
    https://blog.csdn.net/liyantianmin/article/details/54673109



##### 死锁的四个必要条件？

1. 互斥条件: 指线程对己经获取到的资源进行排它性使用 ，即该资源同时只由 一个线
   程占用。如果 此时 还有其 他 线程请求获取该资源 ，则 请求者只能等待，直至占有资
   源 的 线程释放该资源。
2. 请求并持有条件 : 指一个线程己经持有了至少一个资源，但又提出了新的资源请求，
   而新资源己被其 他 线程占有，所 以 当前线程会被阻塞 ，但 阻塞 的同时 并不释放自 己
   己经获取的资源。
3. 不可剥夺条件 : 指线程获取到的资源在自己使用完之前不能被其他线程抢占 ， 只有在自己使用完 毕后才 由 自 己释放该资源。
4. 环路等待条件 : 指在发生死锁时 ，必然存在一个线程→资源的环形链 ，即线程集合

##### 怎么避免死锁？

请求并持有和环路等待条件是可 以被破坏.



##### synchronized是公平锁还是非公平锁,ReteranLock是公平锁吗？是怎么实现的

1. synchronized只能是非公平锁。
2. 而ReentrantLock可以实现公平锁和非公平锁两种。
3. 公平锁的公平的含义在于如果线程现在拿不到这把锁，那么线程就都会进入等待，开始排队，在等待队列里等待时间长的线程会优先拿到这把锁，有先来先得的意思。而非公平锁就不那么“完美”了，它会在一定情况下，忽略掉已经在排队的线程，发生插队现象



##### synchronized跟ReentranLock有什么区别？

都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待

* synchronized

  **自动释放锁**,只有非公平锁。都是可重入的

* ReentrantLock

   **手动释放锁**

##### 对象锁和类锁是否会互相影响？

不会相互影响

* 类锁

  在代码中的方法上加了static和synchronized的锁，或者synchronized(xxx.class）的代码段

* 对象锁

  在代码中的方法上加了synchronized的锁，或者synchronized(this）的代码段

* 私有锁

  在类内部声明一个私有属性如private Object lock，在需要加锁的代码段synchronized(lock）

  

##### 管理线程 提高效率

##### volatile的原理

 有序性.

 可见性 :

　	（1）修改volatile变量时会强制将修改后的值刷新的主内存中。

　　（2）修改volatile变量后会导致其他线程工作内存中对应的变量值失效。因此，再读取该变量值的时候就需要重新从读取主内存中的值。



https://www.cnblogs.com/paddix/p/5428507.html

https://www.bilibili.com/video/BV1NT4y1G7WE?p=8

##### synchronized 和volatile 关键字的区别

1. volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性
2. volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化.



多线程断点续传原理

其实原理很简单，只需要保证每个子任务的下载进度能够被即时地记录即可。这样继续下载时只需要读取这些下载记录，从上次下载结束的位置开始下载即可。

https://juejin.cn/post/6844904013440221198




### 线程池

##### JavaAPI线程池有哪些参数

![](https://s0.lgstatic.com/i/image2/M01/AD/A3/CgoB5l3eH8mAAoJCAACEOKMHtpw036.png)



![](https://s0.lgstatic.com/i/image2/M01/AD/A3/CgoB5l3eH-KAAHpkAAC4vEMOXQ4797.png)



##### 为什么要用线程池

1. 降低资源消耗2. 提高响应速度3. 提高线程的可管理性

##### 什么是线程池，如何使用?

很多小任务让一组线程来执行，而不是一个任务对应一个新线程。这种能接收大量小任务并进行分发处理的就是线程池

##### 什么是核心线程

​	常驻线程池的线程数量

##### 怎么销毁核心线程

​	allowCoreThreadTimeOut

​	https://objcoding.com/2019/04/14/threadpool-some-settings/

##### 为什么DCL DOUBLE CHECK LOCK要那么写，直接在方法前加synchronized不行吗

​	不DCL 这样写，就会创建多个实例

​	synchronized可以，这样粒度太大了.

​	https://blog.csdn.net/zhaoyajie1011/article/details/106812327



##### 如何让两个线程循环交替打印

LockSupport_1A2B.java

```java
  char[] aI = "1234567".toCharArray();
        char[] aC = "ABCDEFG".toCharArray();

        t1 = new Thread(() -> {
            for (char c : aI) {
                System.out.print(c);
                LockSupport.unpark(t2);
                LockSupport.park();
            }
            LockSupport.unpark(t2);
        });
        t2 = new Thread(() -> {
            for (char c : aC) {
                System.out.print(c);
                LockSupport.unpark(t1);
                LockSupport.park();
            }
            LockSupport.unpark(t1);
        });
        t1.start();
        t2.start();
//        System.out.println("t1  " + t1.getState());
//        System.out.println("t2  " + t2.getState());
        t1.join();
        t2.join();
```



Notify_1A2B.java

```java
Notify_1A2B o = new Notify_1A2B();
char[] aI = "1234567".toCharArray();
char[] aC = "ABCDEFG".toCharArray();
t1 = new Thread(() -> {
    synchronized (o) {
        for (char c : aI) {
            System.out.print(c);
            o.notify();
            try {
                o.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

});
t2 = new Thread(() -> {
    try {
        synchronized (o) {
            for (char c : aC) {
                System.out.print(c);
                o.notify();
                o.wait();
            }
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
t1.start();
t2.start();
t1.join();
t2.join();
```



协程可以在Java项目中使用吗？

线程池了解多少？拒绝策略有几种,为什么有newSingleThread

##### 跨进程通信了解多少？管道了解吗？

    文件
    AIDL （基于 Binder）
        Android 进阶：进程通信之 AIDL 的使用
        Android 进阶：进程通信之 AIDL 解析
    Binder
        Android 进阶：进程通信之 Binder 机制浅析
    Messenger （基于 Binder）
        Android 进阶：进程通信之 Messenger 使用与解析 
    ContentProvider （基于 Binder）
        Android 进阶：进程通信之 ContentProvider 内容提供者 
    Socket
        Android 进阶：进程通信之 Socket （顺便回顾 TCP UDP）

![](https://img-blog.csdn.net/20170605011532312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
原文链接：https://blog.csdn.net/u011240877/article/details/72863432



##### 底层原理

- RXJava怎么切换线程
- binder进程间通信可以调用原进程方法吗？
- SharedPreference原理？读取xml是在哪个线程?
- AQS了解吗？
- ConcurrentHashMap，线程安全，为何安全。底层实现是怎么样的。



https://www.zhihu.com/question/63859501



##### AQS

http://gee.cs.oswego.edu/dl/papers/aqs.pdf

https://www.bilibili.com/video/BV11Q4y1M7K2?from=search&seid=12598203519866117819

https://javadoop.com/post/AbstractQueuedSynchronizer

https://www.bilibili.com/video/BV1yJ411v7er?from=search&seid=12598203519866117819



##### 阻塞队列原理

![](https://s0.lgstatic.com/i/image3/M01/62/7D/Cgq2xl4le9SAL6enAAGpXZi8Wcg079.jpg)

阻塞功能:阻塞功能使得生产者和消费者两端的能力得以平衡，当有任何一端速度过快时，阻塞队列便会把过快的速度给降下来

是否有界

*  LinkedBlockingQueue 的上限是 Integer.MAX_VALUE，约为 2 的 31 次方,
* ArrayBlockingQueue 如果容量满了，也不会扩容，所以一旦满了就无法再往里放数据了。

