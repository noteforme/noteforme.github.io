---
title: DeadLocking
comments: true
date: 2018-05-24 22:31:57
tags: concurrency
categories: JAVA
---



#### 死锁

* 条件

1. 互斥条件 :   任务使用的资源至少有一个是不能共享的；chopstick一次只能被一个Philosopher使用
2. 请求保持条件:   至少有一个任务必须持有跟一个资源且正在等待获取一个当前被别的任务持有的资源
3. 不可剥夺条件:  资源不能被任务抢占
4. 环路等待条件:  必须有循环等待。一个任务等待其他任务所持有的资源，后者又等待另一个任务所持有的资源,使得大家都被锁住.



##### 银行家算法



#### 经典的哲学家吃饭问题

 5位哲学家 ，5只筷子在他们之间.    

* 筷子

  ```
  public class Chopstick {
      private boolean taken = false;
  
      public int getNumber() {
          return number;
      }
  
      public void setNumber(int number) {
          this.number = number;
      }
  
      private int number;
  
  
      public synchronized void take(int numChop) throws InterruptedException {
          while (taken) {
              Print.print(numChop+"   wait");
              wait();
          }
          taken = true;
      }
  
      public synchronized void drop() {
          taken = false;
          notifyAll();
      }
  }
  
  ```

  

* 哲学家 

  ```
  public class Philosopher implements Runnable {
      private Chopstick left;
      private Chopstick right;
      private final int id;
      private final int ponderFactor;
      private Random rand = new Random(47);
  
      private void pause() throws InterruptedException {
          if (ponderFactor == 0) return;
          TimeUnit.MILLISECONDS.sleep(rand.nextInt(ponderFactor * 250));
      }
  
      public Philosopher(Chopstick left, Chopstick right, int id, int ponderFactor) {
          this.id = id;
          this.ponderFactor = ponderFactor;
          this.left = left;
          this.right = right;
      }
  
      @Override
      public void run() {
          try {
              while (!Thread.interrupted()) {
                  Print.print(this + " " + "need thinking " + ponderFactor + "    毫秒");
                  pause();
                  //Philosopher becomes hungry
                  Print.print(this + "  " + "grabbing right  第" + (id + 1) + "  号筷子");
                  right.take(id + 1);
  //                if (id == 2) {
  //                    System.out.println(right.taken);
  //                }
                  pause();            //拿起右边的筷子进入等待状态
                  Print.print(this + " ." + "try grabbing left   第  " + (id) + "  号筷子");  //尝试拿起左边的筷子
                  left.take(id);
                  Print.print(this + " " + "eating");
                  pause();
                  right.drop();
                  left.drop();
              }
          } catch (InterruptedException e) {
              Print.print(this + " " + "exiting via interrupt");
          }
      }
  
      @Override
      public String toString() {
          return "Philosopher" + id;
      }
  }
  ```

* 运行

  ```
  public class DeadlockingDiningPhilosophers {
      public static void main(String[] args) throws Exception {
          int ponder = 5; //默认 Philosopher思考的时间
          if (args.length > 0)
              ponder = Integer.parseInt(args[0]);
          int size = 5;  //默认筷子的个数
          if (args.length > 1)
              size = Integer.parseInt(args[1]);
  
          ExecutorService exec = Executors.newCachedThreadPool();
          Chopstick[] sticks = new Chopstick[size];
          for (int i = 0; i < size; i++)
              sticks[i] = new Chopstick();
          for (int i = 0; i < size; i++)
              exec.execute(new Philosopher(sticks[i], sticks[(i + 1) % size], i, ponder));
  
  //        if (args.length == 3 && args[2].equals("timeout"))
          TimeUnit.SECONDS.sleep(5);
  //        else {
  //            System.out.println("Press ''Enter to quit");
  //            System.in.read();
  //        }
  
          exec.shutdownNow();
      }
  }
  ```

  

* 结果

  ```
  Philosopher0 need thinking 5    毫秒
  Philosopher3 need thinking 5    毫秒
  Philosopher2 need thinking 5    毫秒
  Philosopher1 need thinking 5    毫秒
  Philosopher4 need thinking 5    毫秒
  Philosopher0  grabbing right  第1  号筷子
  Philosopher3  grabbing right  第4  号筷子
  Philosopher2  grabbing right  第3  号筷子
  Philosopher1  grabbing right  第2  号筷子
  Philosopher4  grabbing right  第5  号筷子
  Philosopher0 .try grabbing left   第  0  号筷子
  Philosopher3 .try grabbing left   第  3  号筷子
  Philosopher2 .try grabbing left   第  2  号筷子
  Philosopher1 .try grabbing left   第  1  号筷子
  Philosopher4 .try grabbing left   第  4  号筷子
  2   wait
  3   wait
  4   wait
  0   wait
  1   wait
  Philosopher1 exiting via interrupt
  Philosopher4 exiting via interrupt
  Philosopher0 exiting via interrupt
  Philosopher3 exiting via interrupt
  Philosopher2 exiting via interrupt
  ```

  

  

  

  