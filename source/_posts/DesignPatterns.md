---
title: DesignPatterns
comments: true
date: 2017-08-10 08:36:29
tags:
categories: DesignParrerns

---

设计模式：做到高内聚　低耦合

＃＃1.单例模式
　作用：一个类只有一个实例，减少内存开销
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
## 2. Factory（工厂模式）
    工厂类　对实例类进行管理和创建
       public class Factory {
        //静态工厂方法
        public static Cat produceCat() {
            return new Cat();
        }

       public static Dog produceDog() {
            return new Dog();
        }

        public static void main(String[] args) {
            Animal cat = Factory.produceCat();
            cat.move();

            Dog dog = Factory.produceDog();
            dog.move();
            dog.eatBone();
         }
        }`



