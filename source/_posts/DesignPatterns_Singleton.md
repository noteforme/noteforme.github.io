---
title: Singleton
comments: true
date: 2018-06-06 15:43:06
tags:
categories: DesignPattern
---

单例模式

　作用：一个类只有一个实例，减少内存开销
　

#### kotlin

```kotlin
class Singleton private constructor() {

    private object HOLDER {
        val INSTANCE = Singleton()
    }

    companion object {
        val instance: Singleton by lazy { HOLDER.INSTANCE }
    }
}
```

https://medium.com/swlh/singleton-class-in-kotlin-c3398e7fd76b

常用方式

```java
public class Singleton{
     // 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载
     private static Singleton singleton=null;
     //私有构造方法，防止被实例化
     private Singleton(){}
     public static Singleton getInstance(){
     if(singleton==null){           
                                                                  //多个线程在这里聚集就会产生多个对象
           synonzied(Singleton.class){
           Thread.sleel(1)
          singleton = new Singleton();
         }
       }
       return singleton;
     }
   }
```

```java
public class Singleton{
     // 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载
     private static Singleton singleton=null;
     //私有构造方法，防止被实例化
     private Singleton(){}
     public static Singleton getInstance(){  //这里加锁 粒度有问题
      synonzied(Singleton.class){ //这样会产生效率问题，多个线程在这空转等待获得锁
     if(singleton==null){           
           Thread.sleel(1)
          singleton = new Singleton();
         }
       }
       return singleton;
     }
   }
```

源码有类似的　InputMethodManager，当然我们常用的方式是要传个Context上下文对象给单例类，记得有次面试的时候面试官说单例里面使用弱引用，如果是是为了避免内存泄漏我觉得是可以的，但是我觉得用这种方式更好　`Context applicationContext = context.getApplicationContext();`　刚无意中发现Glide单例也是这样使用的。

#### 双重检查模式

```java
public class Singleton {  
      private volatile static Singleton singleton;   //volatile 防止指令重排序,指令半初始化，CPU执行指令顺序可能不同
      private Singleton (){
      }   
      public static Singleton getInstance() {  
      if (singleton== null) {                          
          synchronized (Singleton.class) {  
          if (singleton== null) {  
              instance= new Singleton();  
          }  
         }  
     }  
     return singleton;  
     }  
 }  
```

##### 第1个if (singleton==null)

如果去掉它，那么所有线程都会串行执行，效率低下，这样会产生效率问题，多个线程在这空转等待获得锁，所以两个 check 都是需要保留的。

##### 第2个if (singleton==null)

假如两个线程同时调用 getInstance() ,由于instance是空的，两个线程都通过**第1个if (singleton null)**,接着锁机制存在，线程1先进入同步语句，并进入第二重if判断,线程2在外面等待. 线程1执行完 `new Singleton()`后退出synchronized,这时候如果没有 **第2个if (singleton== null)**  线程2也会创建一个实例，此时就破坏了单例原则.

##### volatile作用

防止 new Singleton()重排序

在 JVM 中上述语句至少做了以下这 3 件事

![](https://s0.lgstatic.com/i/image3/M01/7E/CC/Cgq2xl6BpWCAMBaVAACFIdffjfM852.png)

1. 第一步是给 singleton 分配内存空间；
2. 然后第二步开始调用 Singleton 的构造函数等，来初始化 singleton；
3. 最后第三步，将 singleton 对象指向分配的内存空间（执行完这步 singleton 就不是 null 了）。

因为存在指令重排序的优化，也就是说第2 步和第 3 步的顺序是不能保证的，最终的执行顺序，可能是 1-2-3，也有可能是 1-3-2。

如果是 1-3-2，那么在第 3 步执行完以后，singleton 就不是 null 了，可是这时第 2 步并没有执行，singleton 对象未完成初始化，它的属性的值可能不是我们所预期的值。假设此时线程 2 进入 getInstance 方法，由于 singleton 已经不是 null 了，所以会通过第一重`if (singleton==null) `检查并直接返回，但其实这时的 singleton 并没有完成初始化，所以使用这个实例的时候会报错.

![](https://s0.lgstatic.com/i/image3/M01/7E/CC/Cgq2xl6BpWCAB6QQAAEKacFd0CE542.png)

#### Enum方式

和公有域方法在功能上相近，但是更简洁，无偿提供了序列化机制，绝对的防止多次实例化，可以面对复杂的序列化或者反射攻击，虽然没有广泛采用，但是　单元素的枚举类型
已经成为实现　Singleton的最佳方法.

```java
public class Elvis_03 {
    public static void main(String[] args) {
        Elvis.INSTANCE.leaveTheBuilding();
    }
}
/**
 * 这种方法太NB了,
 * Effective Java pag15
 */
enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() {

    }
}
```

#### 嵌套类

```java
public class Singleton3 {

    private Singleton3() {}
    // 主要是使用了 嵌套类可以访问外部类的静态属性和静态方法 的特性
    private static class Holder {
        private static Singleton3 instance = new Singleton3();
    }
    public static Singleton3 getInstance() {
        return Holder.instance;
    }
}
```

https://juejin.im/post/5bc96afff265da0aa94a4493