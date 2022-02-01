---
title: resume
comments: true
date: 2020-07-15 10:29:39
tags: inter
categories: 
---



https://juejin.im/post/5e801a49e51d45470e2bc8bf

https://wanandroid.com/blog/show/2754

 https://mp.weixin.qq.com/s/AJ4QUqdYMeQWgWEQAFPsww

https://juejin.im/post/6876968255597051917



#### APP模式 MVC MVP. MVVM:

面试题

* mvp与mvvm的区别，mvvm怎么更新UI

* 讲讲mvc,mvp模式，presenter内存泄漏的问题

* 代理模式与装饰模式的区别，手写一个静态代理，一个动态代理

* MVP怎么处理内存泄漏

* **Mvp与Mvvm有什么区别?**

* mvvm双向数据绑定的原理是怎样的？ViewModel

* 你在项目中有用到什么设计模式吗？

* 单例模式有什么缺点？

* 动画里面用到了什么设计模式？

* OkHttp里面用到了什么设计模式？

* 谈谈设计模式，你了解多少，运用了多少？

  

* MVP

  Presenter 的作用类似于MVC中的Controller,但是其会反作用与View层，Model层的数据更新会被首先反馈到

  Presenter,由Presenter优先处理并决定是否刷新以及刷新哪个View,也就是说Presenter完全将View层与Model层

  隔离开，充当一个名副其实的中间人角色.

* MVVM

  MVVM则是思想的完全变革。它是将“数据模型数据双向绑定”的思想作为核心，因此在View和Model之间没有联系，通过ViewModel进行交互，而且Model和ViewModel之间的交互是双向的，因此视图的数据的变化会同时修改数据源，而数据源数据的变化也会立即反应到View上

  缺点

  1、过于简单的图形界面不适用，或说牛刀杀鸡。
  2、对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高。
  3、数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的。



#### 设计模式

##### 原则

1. 设计模式六大原则1-单一职责原则

2. 设计模式六大原则2-里氏替换原则

   > 子类必须实现父类的抽象方法，但不得重写（覆盖）父类的非抽象（已实现）方法，里氏替换原则规定，子类不能覆写父类已实现的方法。父类中已实现的方法其实是一种已定好的规范和契约，如果我们随意的修改了它，那么可能会带来意想不到的错误。
   >
   > 
   >
   > 子类完美继承父类的设计初衷，并做了增强

   https://www.jianshu.com/p/cf9f3c7c0df5

   

3. 设计模式六大原则3-依赖倒置原则

   > 高层模块不应该依赖于底层模块，两者都应该依赖于抽象，抽象不应该依赖于细节，细节应该依赖于抽象
   >
   
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



