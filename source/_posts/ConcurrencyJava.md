---
title: ConcurrencyJava
comments: true
date: 2017-08-27 10:40:55
tags: concurrency
categories: JAVA

---





<img src="ConcurrencyJava/Screen Shot 2020-08-19 at 10.07.23 AM.png" alt="Screen Shot 2020-08-19 at 10.07.23 AM" style="zoom:50%;" />



##### ThinkInJava

###### 并发

让步(yield)

  如果已经完成了run()方法循环的一次迭代过程所需的工作，可以给线程调度机制一个暗示：你的工作已经做的差不多了，可以让别的线程使用CPU了，这个暗示將通过调用yield()来做做出(不过这只是一个暗示，没有任何机制保证它將被采纳)，也只是建议相同优先级的其他线程运行。
  -ThinkInJava P661

加入一个线程 (join)

　在Joiner线程里面调用Sleeper线程 的join() , Joiner任务必须等Sleeper任务结束活被打断或结束　才恢复

```
class Sleeper extends Thread {
    private int duration;

    public Sleeper(String name, int sleepTime) {
        super(name);
        duration = sleepTime;
        start();
    }

    @Override
    public void run() {
        super.run();
        try {
            sleep(duration);
//            Print.print(getName()+"执行了");
        } catch (InterruptedException e) {
            Print.print(getName() + "  被打断" + "isInterrupted()  " + isInterrupted());
            return;
        }
        Print.print(getName() + "   has awakened");
    }
}

class Joiner extends Thread {
    private Sleeper sleeper;

    public Joiner(String name, Sleeper sleeper) {
        super(name);
        this.sleeper = sleeper;
        start();
    }

    @Override
    public void run() {
        super.run();
        try {
            sleeper.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Print.print(getName() + "   join completed");
    }
}

public class Joining {
    public static void main(String[] args) {
        Sleeper
                sleepy = new Sleeper("Sleepy", 1500),
                grumpy = new Sleeper("Grumpy", 1500);

        Joiner
                dopey = new Joiner("Dopey", sleepy),
                doc = new Joiner("Doc", grumpy);
        grumpy.interrupt();

```

其他对象上同步

 有时候必须在另一个对象上同步，如果需要这样，必须确保所有相关的任务都是在同一个对象上同步。
```
class DualSynch {
    private Object syncObject = new Object();

    public synchronized void f() {
        for (int i = 0; i < 5; i++) {
            System.out.print("  f()  ");
            Thread.yield();
        }
    }

    public  void g() {
        synchronized (syncObject) {
            for (int i = 0; i < 5; i++) {
                System.out.print("  g()  ");
                Thread.yield();
            }
        }
    }
}

public class SyncObject {
    public static void main(String[] args) {
        DualSynch ds = new DualSynch();
        new Thread() {
            @Override
            public void run() {
                super.run();
                ds.f();
            }
        }.start();
        ds.g();
    }
}
```
这两个方式在同时运行，任何一个方法都没有对另一个方法同步而阻塞

线程间的协作

- wait()
   : 在wait期间　对象锁是释放的,而Sleep期间是没有的

##  

CountDownLatch

https://www.jianshu.com/p/cef6243cdfd9

latch.countDown();//完成一个任务就调用一次



#####  学习方法

https://mp.weixin.qq.com/s/BHDrSgwUVXkzvswK1khidQ

https://github.com/Snailclimb/programmer-advancement

https://juejin.im/post/5a743c526fb9a063557d7eba



https://juejin.im/post/6855586076132655118



#### 并发编程之美

9 

//线程A阻塞 ，并释放获取到的 resourceA的锁

System.out.println (”threadA release resourceA lock"); 

resourceA .wait() ; //并释放获取到的 resourceA的锁? 为什么会释放



#### 死锁

##### 死锁条件

两个或两个以上的线程在执行过程中，因争夺资源而造成的相互等待的现象.



那么为什么会产生死锁呢? 学过操作系统的朋友应该都知道，死锁的产生必须具备以
下四个条件 。

1. 互斥条件: 指线程对己经获取到的资源进行排它性使用 ，即该资源同时只由 一个线
   程占用。如果 此时 还有其 他 线程请求获取该资源 ，则 请求者只能等待，直至占有资
   源 的 线程释放该资源。
2. .请求并持有条件 : 指一个线程己经持有了至少一个资源，但又提出了新的资源请求，
   而新资源己被其 他 线程占有，所 以 当前线程会被阻塞 ，但 阻塞 的同时 并不释放自 己
   己经获取的资源。
3. 不可剥夺条件 : 指线程获取到的资源在自己使用完之前不能被其他线程抢占 ， 只有在自己使用完 毕后才 由 自 己释放该资源。
4. 环路等待条件 : 指在发生死锁时 ，必然存在一个线程→资源的环形链 ，即线程集合
   {TO,TLT2，...，Tn}中的TO正在等待一个Tl占用的资源， Tl正在等待T2占 用的资源，......Tn正在等待己被 TO 占用的资源。

##### 避免死锁

##### 要想避免死锁，只需要破坏掉至少一个构造死锁的必要条件即可， 但是学过操作系统
的读者应该都知道，目前只有请求并持有和环路等待条件是可 以被破坏 的。
造成死锁的原因其实和申请资源的顺序有很大关系 ，



#### 共享变量可见性问题

<img src="ConcurrencyJava/Screen Shot 2020-08-19 at 3.54.13 PM.png" alt="Screen Shot 2020-08-19 at 3.54.13 PM" style="zoom:67%;" />

图中所示是一个双核 CPU系统架构，每个核有自己的控制器和运算器，其中控制器包含 一组寄 存器和操作控制 器，运算器执 行算术逻辅运算 。每 个核都有自己的 一 级缓存，在有些架构里面还有一个所有 CPU 都共享的二级缓存。 那么 Java 内存模型里面的工作内
存，就对应这里的 Ll 或者 L2 缓存或者 CPU 的寄存器。



当一个线程操作共享变量时， 它首先从主内存复制共享变量到自己的工作内存， 然后
对工作 内存里 的变量进行处理，
那么假如线程 A 和线程 B 同时处理一个共享变量 ，
处理完后将变量值更新到主 内存。
所示CPU架构， 假设线程A和线程B使用不同CPU执行，并且当前两级Cache都为空，
那么这时候由于 Cache 的存在，将会导致内存不可见 问题 ，
具体看下面的分析。
· 线程A首先获取共享变量X的值，由于两级Cache都没有命中，所以加载主内存
中 X 的值，假如为 0。然后把 X=O 的值缓存到两级缓存 ，
然后将其写入两级 Cache，
并且刷新到主内存 。
线程 A 修改 X 的值为 1,
线程 A 操作完毕后，线程 A 所在的
CPU 的两级 Cache 内和主内存里面的 X 的值都是 l。
· 线程 B 获取 X 的值，首先一级缓存没有命中，然后看二级缓存，二级缓存命中了 ，
所以返回X=1; 到这里一切都是正常的， 因为这时候主内存中也是X=l。然后线 程 B 修改 X 的值为 2， 并将其存放到线程 2 所在 的 一 级 Cache 和共享二级 Cache 中， 最后更新主内存中X的值为2: 到这里一切都是好的。
· 线程A这次又需要修改X的值， 获取时一级缓存命中， 并且X=l，到这里问题就





Sychronized volatile区别？

https://crowhawk.github.io/2018/02/10/volatile/ 

https://juejin.im/post/6864252499466354701