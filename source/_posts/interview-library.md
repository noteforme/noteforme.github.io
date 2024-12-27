---
id: 152
title: INTERVIEW-LIABARY
date: '2024-05-22T06:39:05+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=152'
permalink: /2024/05/22/interview/
categories:
    - INTERVIEW
---



https://github.com/pengMaster/BestNote/blob/master/docs/android/Android-Interview/Android/Android%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%9510%E5%A4%A7%E5%BC%80%E6%BA%90%E6%A1%86%E6%9E%B6%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90.md

https://www.youtube.com/watch?v=pUV3qZh481k

# okhttp

https://jonblog.site/2024/05/11/okhttp/

https://www.51cto.com/article/689330.html

      OKHttp 请求的整体流程是怎样的?
      OKHttp 分发器是怎样工作的?
      OKHttp 拦截器是如何工作的?
      应用拦截器和网络拦截器有什么区别?
      OKHttp 如何复用 TCP 连接?
      OKHttp 空闲连接如何清除?
      OKHttp 有哪些优点?
      OKHttp 框架中用到了哪些设计模式?
      okhttp怎么支持http2.0 
       配置合适的适配器，解析json数据。
      Android 如何编写基于编译时注解的项目
    编译时注解与运行时注解，为什么retrofit要使用运行时注解？什么时候用运行时注解？
    在项目中有直接使用tcp,socket来发送消息吗
    
    https://www.bilibili.com/video/BV1ib4y1f7S1

* okhttp怎么支持http2.0
    Handshake则会把服务端支持的Tls版本，加密方式等都带回来，然后会把这个没有验证过的HandShake用X509Certificate去验证证书的有效性。然后会通过Platform去从SSLSocket去获取ALPN的协议支持信息，当后端支持的协议内包含Http2.0时，则就会把请求升级到Http2.0阶段。

* 配置合适的适配器，解析json数据。

* Android 如何编写基于编译时注解的项目

* 编译时注解与运行时注解，为什么retrofit要使用运行时注解？什么时候用运行时注解？
  
  https://www.bilibili.com/video/BV1ib4y1f7S1

Okhttp缓存机制

网络请求缓存处理，okhttp如何处理网络缓存的

自己去设计网络请求框架，怎么做？

HTTP

# Network

- 从网络加载一个10M的图片，说下注意事项
- TCP的3次握手和四次挥手
- TCP与UDP的区别
- TCP与UDP的应用
- HTTP协议
- HTTP1.0与2.0的区别
- HTTP报文结构
- HTTP与HTTPS的区别以及如何实现安全性
- 如何验证证书的合法性?
- https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
- client如何确定自己发送的消息被server收到?
- 谈谈你对安卓签名的理解。
- 请解释安卓为啥要加签名机制?
- 视频加密传输
- App 是如何沙箱化，为什么要这么做？
- 权限管理系统（底层的权限是如何进行 grant 的）？

从网络加载一个10M的图片，说下注意事项 TCP的3次握手和四次挥手 TCP与UDP的区别 TCP与UDP的应用 HTTP协议 HTTP1.0与2.0的区别 HTTP报文结构 HTTP与HTTPS的区别以及如何实现安全性 如何验证证书的合法性? https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解? client如何确定自己发送的消息被server收到? 谈谈你对安卓签名的理解。 请解释安卓为啥要加签名机制? 视频加密传输 App 是如何沙箱化，为什么要这么做？ 权限管理系统（底层的权限是如何进行 grant 的）？

# okio

1.简介； 1.sink：自己–》别人 2.source：别人–》自己 3.BufferSink：有缓存区域的sink 4.BufferSource：有缓存区域的source 5.Buffer：实现了3、4的缓存区域，内部有Segment的双向链表，在在转移数据的时候，只需要将指针转移指向就行 2.比java io的好处： 1.减少内存申请和数据拷贝 2.类少，功能齐全，开发效率高 3.内部实现： 1.Buffer的Segment双向链表，减少数据拷贝 2.Segment的内部byte数组的共享，减少数据拷贝 3.SegmentPool的共享和回收Segment 4.sink和source中被实际操作的其实是Buffer，Buffer可以充当sink和source 5.最终okio只是对java io的封装，所有操作都是基于java io 的 写在最后:能看到这里的人,我挺佩服你的.这篇文章是我在头条面试之前整理的,最后**80%**的题目都命中了,所以祝你好运.

不贩卖焦虑，也不标题党。分享一些这个世界上有意思的事情。题材包括且不限于：科幻、科学、科技、互联网、程序员、计算机编程。下面是我的微信公众号：世界上有意思的事，干货多多等你来看。

作者：何时夕 链接：[https://www.jianshu.com/p/cf5092fa2694](https://www.jianshu.com/p/cf5092fa2694)来源：简书 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# Retrofit

在retrofit中的泛型是怎么解析的

https://noteforme.github.io/2017/09/23/Retrofit/

# coroutine 面试

kotlin协程有没有了解过。（协程是kotlin 语言对线程的封装）
协程实现原理
协程的理解
协程相对于线程的区别
协程Flow有哪些应用场景？
进程、线程、协程的联系与区别 
你了解协程吗？协程有什么作用？可以完全取代rxjava吗？

 高端技术面试题

**这里讲的是大公司需要用到的一些高端Android技术，这里专门整理了一个文档，希望大家都可以看看。这些题目有点技术含量，需要好点时间去研究一下的。**

# Glide

https://jonblog.site/2024/06/18/glide/

# RxJava

https://noteforme.github.io/2020/08/04/RxJava/

在 RxJava 中， 会遇到被观察者发送消息太快以至于它的操作符或者订阅者不能及时处理相关的消息，这就是典型的背压（ Back Pressure ）场景。

RxJava 是基于 Push 模型 。对于 Pull 模型而言，当消费者请求数据的时候，如果生产者比较慢 ，则消费者会阻塞等待。如果生产者比较快，生产者会等待消费者处理完后再生产新的数据，所以不会出现背压的情况。然而在 RxJava 中，只要生产者数据准备好了就会发射出去。如果生产者比较慢，则消费者会等待新的数据到来。如果生产者比较快，则会有很多数据发射给消费者，而不管消费者当前有没有能力处理数据，这样就会导致背压。

在 RxJava 2.x 中， Observable 不再支持背压，而是改用 Flowable 来专门支持背压。默认队列大小为 128 ，并且要求所有的操作符强制支持背压。

### RxJava原理及如何封装使用

​ 使用观察者模式，和装饰器模式。

​ 先用Observablex向右构建流，然后向左创建订阅流，最后时间通过观察者回调流发送。

​

​ 你了解协程吗？协程有什么作用？可以完全取代rxjava吗？

### rxjava里面用了大量的`<? super T>`这些，是什么意思。

- 如果要从集合中读取类型T的数据，并且不能写入，可以使用 ? extends 通配符；(Producer Extends)

- 如果要从集合中写入类型T的数据，并且不需要读取，可以使用 ? super 通配符；(Consumer Super),只能用Object接收。

- 如果既要存又要取，那么就不要使用任何通配符。
  
  https://www.jianshu.com/p/86cf908afcb6

### RxJava怎么通过被订阅者传给订阅者的过程是什么样的?

### Observer处理完onComplete后会还能onNext吗?

​ onComplete是用来控制不能发送数据的，也就是不能onNext了，包括onError也是不能再发送onNext数据了，该方法中也是调用了 dispose方法。

### RxJava中map、flatMap的区别，你还用过其他哪些操作符?

​ map是通过原始数据类型返回另外一种数据类型，而flatMap是通过原始数据类型返回另外一种被观察者。

### Maybe、Single、Flowable、Completable几种观察者的区别，以及他们在什么场景用？

​ 如果只想发一条数据，或者不发数据就用Maybe，如果想发多条数据或者不发数据就用Observable，如果只发一条数据或者失败就用Single，如果想用背压策略使用Flowable，如果不发数据就用Completable

也就是说Maybe可能不发送数据，如果发送数据只会发送单条数据。

single也是发送单条数据，但是它要么成功要么失败。

### RxJava切换线程是怎么回事?

subscribeOn实际是创建了ObservableSubscribeOn的Observable，它的订阅方法里面创建了`SubscribeOnObserver`，通过线程池执行Runnable，使上游Observable的订阅在子线程中执行，这就是为什么subscribeOn能控制observable在哪个线程中执行的原因

### RxJava的subscribeOn只有第一次生效?

subscribeOn对subscribe订阅进行处理，针对是订阅流，从后向前流动，所以最前面的一次生效。

subscribeOn是规定上游的observable在哪个线程中执行，如果我们执行多次的subscribeOn的话，从下游的observer到上游的observable的订阅过程，**最开始调用的subscribeOn返回的observable会把后面执行的subscribeOn返回的observable给覆盖了，因此我们感官的是只有第一次的subscribeOn能生效。**

### RxJava的observeOn多次调用哪个有效?

observeOn在事件发送的 onNext(T t)进行处理，针对的是观察者流，从前向后流动，所以最后一次生效。

observeOn是指定下游的observer在哪个线程中执行，所以这个更好理解，看observeOn下一个observer是哪一个，**所以多次调用observeOn肯定是最后一个observeOn控制有效。**

https://juejin.cn/post/6900870262062120967

https://zhuanlan.zhihu.com/p/339620311

RxJava中背压是怎么回事？

https://www.jianshu.com/p/c3965e82b164

https://zhuanlan.zhihu.com/p/322405376

https://www.bilibili.com/video/BV1Af4y187b8

# 数据库

- sqlite升级，增加字段的语句
- 数据库框架对比和源码分析
- 数据库的优化
- 数据库数据迁移问题

# 插件化、模块化、组件化、热修复、增量更新、Gradle

- 对热修复和插件化的理解
- 插件化原理分析
- 模块化实现（好处，原因）
- 热修复,插件化
- 项目组件化的理解
- 描述清点击 Android Studio 的 build 按钮后发生了什么

# 架构设计和设计模式

​ https://noteforme.github.io/categories/DesignPatterns/

# 设计原则

​ 单一职责

​ 一个类应该只负责一项职责。

接口隔离

​ 客户端不应该依赖它不需要的接口，即一个类对另一个类的依赖应该建立在最小的接口上。

依赖倒置

- 高层模块不应该依赖低层模块，二者都应该依赖其抽象。
- 抽象不应该依赖细节，细节应该依赖抽象。
- 依赖倒置的中心思想是**面向接口编程**。

里氏替换

- 所有使用基类的地方必须能透明的使用其子类。
- 使用继承时，遵循里氏替换原则，**子类尽量不要重写父类的方法**。继承实际上让两个类耦合性增强了，适当情况下可以通过 聚合 组合 依赖来解决问题。

​ 通用做法是： 原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖，聚合，组合等关系代替。

开闭原则 ocp

迪米特法则

​ 一个对象应该对其他对象保持最少的了解

# 设计模式

- 谈谈你对Android设计模式的理解
- MVC MVP MVVM原理和区别
- 你所知道的设计模式有哪些？
- 项目中常用的设计模式
- 手写生产者/消费者模式
- 写出观察者模式的代码
- 适配器模式，装饰者模式，外观模式的异同？
- 用到的一些开源框架，介绍一个看过源码的，内部实现过程。
- 谈谈对RxJava的理解
- RxJava的功能与原理实现
- RxJava的作用，与平时使用的异步操作来比的优缺点
- 说说EventBus作用，实现方式，代替EventBus的方式
- 从0设计一款App整体架构，如何去做？
- 说一款你认为当前比较火的应用并设计(比如：直播APP，P2P金融，小视频等)
- 谈谈对java状态机理解
- Fragment如果在Adapter中使用应该如何解耦？
- Binder机制及底层实现
- 对于应用更新这块是如何做的？(解答：灰度，强制更新，分区域更新)？
- 实现一个Json解析器(可以通过正则提高速度)
- 统计启动时长,标准

# 性能优化

- 如何对Android 应用进行性能分析以及优化?
- ddms 和 traceView
- 性能优化如何分析systrace？
- 用IDE如何分析内存泄漏？
- Java多线程引发的性能问题，怎么解决？
- 启动页白屏及黑屏解决？
- 启动太慢怎么解决？
- 怎么保证应用启动不卡顿？
- App启动崩溃异常捕捉
- 自定义View注意事项
- 现在下载速度很慢,试从网络协议的角度分析原因,并优化(提示：网络的5层都可以涉及)。
- Https请求慢的解决办法（提示：DNS，携带数据，直接访问IP）
- 如何保持应用的稳定性
- RecyclerView和ListView的性能对比
- ListView的优化
- RecycleView优化
- View渲染
- Bitmap如何处理大图，如一张30M的大图，如何预防OOM
- java中的四种引用的区别以及使用场景
- 强引用置为null，会不会被回收？

# NDK、jni、Binder、AIDL、进程通信有关

- 请介绍一下NDK
- 什么是NDK库?
- jni用过吗？
- 如何在jni中注册native函数，有几种注册方式?
- Java如何调用c、c++语言？
- jni如何调用java层代码？
- 进程间通信的方式？
- Binder机制
- 简述IPC？
- 什么是AIDL？
- AIDL解决了什么问题？
- AIDL如何使用？
- Android 上的 Inter-Process-Communication 跨进程通信时如何工作的？
- 多进程场景遇见过么？
- Android进程分类？
- 进程和 Application 的生命周期？
- 进程调度
- 谈谈对进程共享和线程安全的认识
- 谈谈对多进程开发的理解以及多进程应用场景
- 什么是协程？

# framework层、ROM定制、Ubuntu、Linux之类的问题

- java虚拟机的特性
- 谈谈对jvm的理解
- JVM内存区域，开线程影响哪块内存
- 对Dalvik、ART虚拟机有什么了解？
- Art和Dalvik对比
- 虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)
- 谈谈你对双亲委派模型理解
- JVM内存模型，内存区域
- 类加载机制
- 谈谈对ClassLoader(类加载器)的理解
- 谈谈对动态加载（OSGI）的理解
- 内存对象的循环引用及避免
- 内存回收机制、GC回收策略、GC原理时机以及GC对象
- 垃圾回收机制与调用System.gc()区别
- 系统启动流程是什么？（提示：Zygote进程 –> SystemServer进程 –> 各种系统服务 –> 应用进程）
- 大体说清一个应用程序安装到手机上时发生了什么
- 简述Activity启动全部过程
- App启动流程，从点击桌面开始
- Android中进程内存的分配，能不能自己分配定额内存？
- 如何保证一个后台服务不被杀死？（相同问题：如何保证service在后台不被kill？）比较省电的方式是什么？
- App中唤醒其他进程的实现方式

​ https://www.jianshu.com/p/c3965e82b164

​ https://www.cnblogs.com/deman/p/5860976.html#_label29

- **AMS是如何启动的？**
- **AMS在Android起到什么作用？**
- **AMS有哪些应用场景？我们是如何应用AMS核心原理的？**
- **WMS的工作原理说说？**
- **JVM的核心原理你懂多少？**
- **我们的代码是如何在栈区中运行的？**
- **如何使用字节码研究系统级原理？

[大佬面试经验](https://www.1point3acres.com/bbs/interview/airbnb(%E5%8C%97%E4%BA%AC)%E3%80%81%E5%BF%AB%E6%89%8B%E3%80%81%E5%B0%8F%E7%BA%A2%E4%B9%A6%E3%80%81%E7%8C%BF%E9%A2%98%E5%BA%93%E7%AD%8915%E5%AE%B6%E9%9D%A2%E7%BB%8F-mobile-ios-android-530393.html)

https://interview-q-a-1gdnkgkla15afdbe-1258598664.tcloudbaseapp.com/Android/