---
title: Singleton
comments: true
date: 2018-07-06 15:43:06
tags:
categories: DesignPatterns
---

# 单例模式

　作用：一个类只有一个实例，减少内存开销
　

## 常用方式

```
public class Singleton{
     // 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载
     private static Singleton singleton=null;
     //私有构造方法，防止被实例化
     private Singleton(){}
     public static Singleton getInstance(){
       synonzied(Singleton.class){
         if(singleton==null){
          singleton = new Singleton();
         }
       }
       return singleton;
     }
   }

```

源码有类似的　InputMethodManager，当然我们常用的方式是要传个Context上下文对象给单例类，记得有次面试的时候面试官说单例里面使用弱引用，如果是是为了避免内存泄漏我觉得是可以的，但是我觉得用这种方式更好　`Context applicationContext = context.getApplicationContext();`　刚无意中发现Glide单例也是这样使用的。

## Enum方式

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



