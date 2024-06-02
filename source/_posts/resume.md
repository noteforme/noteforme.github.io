---
title: resume
comments: true
date: 2020-07-15 10:29:39
tags: inter
categories: 
---



#### 设计模式

##### 原则

1. 设计模式六大原则1-单一职责原则

2. 设计模式六大原则2-里氏替换原则
   
   > 子类必须实现父类的抽象方法，但不得重写（覆盖）父类的非抽象（已实现）方法，里氏替换原则规定，子类不能覆写父类已实现的方法。父类中已实现的方法其实是一种已定好的规范和契约，如果我们随意的修改了它，那么可能会带来意想不到的错误。
   > 
   > 子类完美继承父类的设计初衷，并做了增强
   
   https://www.jianshu.com/p/cf9f3c7c0df5

3. 设计模式六大原则3-依赖倒置原则
   
   > 高层模块不应该依赖于底层模块，两者都应该依赖于抽象，抽象不应该依赖于细节，细节应该依赖于抽象

4. 设计模式六大原则4-接口隔离原则
   
   接口隔离原则:Clients should not be forced to depend upon interfaces that they do not use.与外部关系上只依赖需要的抽象
   
   > 接口隔离原则的关键是接口以及这个接口要小，如何小呢，也就是我们要为专门的类创建专门的接口，这个接口只对它有效，不要试图让一个接口包罗万象，要建立最小的依赖关系
   
   https://blog.csdn.net/dingshuo168/article/details/103531805

5. 设计模式六大原则5-迪米特法则
   
   减少依赖 Each unit should have only limited knowledge about other units: only units “closely” related to the current unit. Or: Each unit should only talk to its friends; Don’t talk to strangers
   
   > 一个类应该应该对其他类尽可能了解得最少；类只与直接的朋友通信等等。但是其最终目的只有一个，就是让类间解耦。
   
   https://tianweili.github.io/2015/02/12/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E5%85%AD%E5%A4%A7%E5%8E%9F%E5%88%99-%E8%BF%AA%E7%B1%B3%E7%89%B9%E6%B3%95%E5%88%99/

6. 设计模式六大原则6-开闭原则

7. 控制反转
   
   在使用框架之后，整个程序的执行流程通过框架来控制。流程的控制权从程序员“反转”给了框架。

谈一谈自己对依赖、关联、聚合和组合之间区别的理解

http://blog.itpub.net/69952849/viewspace-2672009/

##### 设计模式之美

迪米特法则的描述为：不该有直接依赖关系的类之间，不要有依赖；有依赖关系的类之间，尽量只依赖必要的接口。

https://www.jianshu.com/p/35f76e87ac45

https://juejin.im/post/6844903591686176776

 https://github.com/Meng997998/AndroidJX

https://juejin.im/post/6864252499466354701

https://github.com/JsonChao/Awesome-Android-Interview

https://juejin.im/post/6878902981400625160

#### 并发

单例为什么使用volatile修饰 

https://juejin.im/post/6844903605292498958

http://static.kancloud.cn/alex_wsc/mianshi/1811436

https://mp.weixin.qq.com/s/eCotdGOWvSMki062eLjS8g

https://mp.weixin.qq.com/s/7DvReHYugl1KClKFGBwSfg

* 平常有用到什么锁，synchronized底层原理是什么
* 锁之间的区别
* 线程间同步的方法
* 阿里编程规范不建议使用线程池，为什么？
* RXJava怎么切换线程
* 平常有用到什么锁，synchronized底层原理是什么
* 简单描述下Handler,Handler是怎么切换线程的,Handler同步屏障
* synchronized是公平锁还是非公平锁,ReteranLock是公平锁吗？是怎么实现的
* 线程间同步的方法
* 锁之间的区别
* OkHttp怎么实现连接池
* 如果让你来实现一个网络框架，你会考虑什么
* 说说你对volatile字段有什么用途？
* 四种线程池原理？
* 怎么中止一个线程，Thread.Interupt一定有效吗？
* 如何让两个线程循环交替打印
* 线程池了解多少？拒绝策略有几种,为什么有newSingleThread
* 

okhttp线程使用方式

#### RXJava

* RXJava怎么切换线程
* Rxjava自定义操作符

##### JVM

https://juejin.im/post/6875638406165037063

设计模式之美

25 26 39. 40代码没什么了解

Hashmap 

#### 注解作用

路线图 

注解
