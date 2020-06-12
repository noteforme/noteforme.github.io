---
title: ConcurrencyJava
comments: true
date: 2017-08-27 10:40:55
tags: concurrency
categories: JAVA

---


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