---
title: Singleton
comments: true
date: 2018-06-06 15:43:06
tags:
categories: DesignPatterns
---

##### 单例模式

　作用：一个类只有一个实例，减少内存开销
　



##### kotlin

```
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

###### 常用方式



```
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



```
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

* 双重检查模式 

  

  

```
public class Singleton {  
      private volatile static Singleton singleton;   //volatile 防止指令重排序,指令半初始化，CPU执行指令顺序可能不同
      private Singleton (){
      }   
      public static Singleton getInstance() {  
      if (instance== null) {  
          synchronized (Singleton.class) {  
          if (instance== null) {  
              instance= new Singleton();  
          }  
         }  
     }  
     return singleton;  
     }  
 }  
```



###### Enum方式

和公有域方法在功能上相近，但是更简洁，无偿提供了序列化机制，绝对的防止多次实例化，可以面对复杂的序列化或者反射攻击，虽然没有广泛采用，但是　单元素的枚举类型
已经成为实现　Singleton的最佳方法.

```
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



######  嵌套类 

```
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

链接：https://juejin.im/post/5bc96afff265da0aa94a4493

```

