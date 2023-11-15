---
title: Android_inerview_organize
date: 2023-09-04 15:54:57
tags: inter

---



# 我的面试

项目的难点，解决

项目中遇到的难题 使用了什么技术来解决

项目中遇到什么困难，应用场景

1. MVP MVVM,现在组件演进成MVVM，最主要的原因是什么。

2. 讲下viewmodel,livedata

3. ViewModel为什么不会丢失

4. Livedata版本管理，这块有没有了解.(也就是粘性事件)

5. kotlin协程有没有了解过。（协程是kotlin     语言对线程的封装）

6. 讲下java匿名内部类

7. Kotlin     内部类的实现方式。

8. kotlin拓展函数是如何做到的。

9. 为什么https要这么麻烦呢。

10. Https 服务端公钥客户端认可，如何做到的

11. 中间人怎么攻击怎么做。

12. 反射的原理，为什么能获取方法。

13. 讲下阻塞队列，怎么实现。

14. 对flutter怎么看.

15. 阻塞队列原理

16. 组件化用过没，组件化用什么通信，arouter底层源码

Arouter讲的不清楚

1. Android用了哪些第三方库

2. Retrofit底层实现，在okhttp的基础下做了哪些封装

3. Okhttp     Resonse之后怎么交给Rxjava处理

4. 自定义view绘制流程

5. 1. 构造方法 -      onsizechange - onmeasre - ondraw
   2. 说下Onmeasrstate      三个常量 atmost作用

6. 为什么onMeasrue     onlayout 绘制的过程执行多遍，ondraw不会

7. 1. 自定义view宽高写死，不管什么地方宽高一样的，xml使用

8. Glide使用什么缓存机制

9. LRUcache底层实现

10. 讲下事件分发机制

11. 事件分发都没拦截，最终被谁消费

12. hashmap hashtable区别

13. Hashmap扩容机制 负载因子

14. Activity启动过程

15. Aws作用

16. 属性动画和其他动画的区别

17. 为什么不用系统进程通信用Binder

18. 内存泄漏方式

19. kotlin特点有什么不习惯的地方 ， 我说了并发

20. flutter包为什么那么大

21. fultter有什么特点

22. 数字签名？

23. 谁通知zygote进程，fork新进程。

24. 点击桌面图标，应用启动流程

25. 屏幕上有个button,为什么button能消费事件

26. https流程，     签名是什么,怎么知道服务端是谁。

27. 客户端输入一个地址到服务端，中间经历了哪些流程。

28. 路由器是怎么找服务器的。

29. TCP对包处理做了什么。

30. 客户投诉，反馈的问题是怎么解决，发版本。

31. Intent机制什么样的

32. 发起一个Binder请求，中间有一个怎么样的流程。

33. 多线程是怎么改造的。

34. 线程池里面的内部调度机制是怎么样的，什么情况下让出，什么时候抓住锁。或者说线程之间的状态是怎么切换的。

35. 加密平时有了解吗

36. 快速排序。

37. MVC MVP     MVVM区别

mvvm说得不清楚

1. MVP里面p持有activity引用，怎么持有比较好。
2. MVP Presenter复用，怎么处理比较好。
3. 项目中用到测量模式，语音项目代码结构怎么样。
4. Okhttp retrofit     Rxjava有没有看过源码。
5. OkHttp拦截器有没有用过，拦截器怎么实现整体的拦截的。
6. 讲下Retrofit
7. 动态代理类具体的方法实现。自己在项目里面有没有用过动态代理。
8. 动态代理用到哪些场景。
9. Retrofit里面用Rxjava返回一个Observable，这个是怎么实现的。
10. Retrofit还有convert，这个convert和CallAdabter的区别。
11. 有没有自定义convert没
12. onMeasure的时候，怎么确定view的大小。
13. Xml layout_width     layout_height对应代码里面怎么对应。
14. Https怎么防止中间人攻击。Server的公钥一定要和本地一样的是吧。
15. 性能优化你做过哪些方面的。
16. 内存泄漏是怎么发生的。
17. 内存抖动是用什么工具进行检测的。**内存抖动很频繁的话，有没有针对性的做一些优化****.(****这个最好实践下****)**
18. Android还有一个常见的性能问题，ANR你能说一下吗。
19. 线上的ANR有没有排查的经验。怎么处理的。
20. kotlin在项目中有没有使用过。Kotlin相比java有哪些优势。
21. Kotlin ? !!都可以让编译通过，实际上有区别吗。it对象是怎么来的
22. Findviewbyid     kotlin是怎么实现的。findviewbyid通过Map 缓存,在项目里面有使用过缓存吗。
23. Reprice三个单元测试步骤.
24. 单元测试覆盖率有没有看。
25. 组件化框架，共享数据怎么处理。放公共里面就太多了。用户model需要其他组件使用。这个怎么处理 。
26. 自定义view触摸事件分发，能说一下吗。 
27. ViewGroup拦截了view的事件，怎么让view不拦截，内部拦截法。
28. ScrollView     放了 竖的viewpager 怎么处理滑动冲突。
29. Gradle插件，自动化的任务。

# HTTP

介绍RPC，RPC和其他协议HTTP有什么区别，然后扣项目细节。。。（录像都不敢看，不想写了）

客户端网络安全实现

HTTP1.0、HTTP1.1、HTTP2.0知道吗？他们相比之前的版本有哪些优化？

输入一个URL到网页显示的过程 3 

(输入完了直接弹出一个广告可能是哪个环节出了问题，怎么解决
我猜是DNS解析出了问题，不知道怎么解决 面完之后想到清浏览器缓存、加强前端校验之类的)

localhost 与127.0.0.1的区别

HTTP1，HTTP2，HTTP3的协议

get与post区别

HTTPS的加密过程，为什么第一次发送公钥不需要加密
HTTP状态码分别代表什么意思

HTTPS是怎么实现的？

HTTPS是怎么保证安全的

https请求流程     3

HTTP3 

http的流程以及和https的区别

HTTP支持长链接吗？什么时候开始支持的？

HTTPS的组成是什么？

http状态码

https请求到响应的过程（加密过程）

怎么加速http连接

http基于tcp还是udp

http协议有哪些跟缓存相关的属性？

http可以用UDP吗

OSI七层模型介绍一下？应用层有什么协议？

OSI七层网络模型

网络传输层的协议

传输层的主要协议有哪些

微信视频是用了tcp还是udp

UDP和TCP的区别，UDP的使用例子，TCP怎么保证连接可靠

TCP三次握手四次挥手，如果中间信息丢失了怎么办，设置超时时间那这个时间具体应该设置多少

OSI七层有哪些  TCP UDP在哪些层  交换器路由器在哪些层

TCP拥塞控制

四次挥手详解

TCP怎么保证可靠传输

DNS解析过程

TCP报文里的字段 

TCP报头

UDP STL  和TCP区别 2

TCP与UDP区别，优缺点

TCP传输中header里的字段

TCP三次握手，如何保证安全传输的

TCP三次握手，TCP三次握手的步骤？两次握手为什么不行？ 3

tcp滑动窗口，TCP三次握手  2

TCP拥塞控制

DNS协议 2

5层结构以及相应的作用

长连接什么时候会释放？

服务端通过timeout还是探测决定是否关闭长连接？

TCP有没有这种关闭连接的方式？

对称加密和非对称加密有哪些算法  什么区别

# ANDROID

handler的作用

深入问handler（比如messagequeue为空时候，looper在做什么）

handle和Activity在链表中的顺序是怎么样的？

Handler为什么会发生内存泄漏

looper阻塞为什么不会造成ANR

looper死循环会不会卡死？为什么？

handler详解，是否会内存泄漏，泄露的原理

handler处理流程，looper和handler是一对一还是一对多，为什么主线程loop不会ANR？

广播的两种启动方式

安卓中的消息机制是什么样的 

消息机制中，如何更新UI 

安卓有哪几种页面通讯的方式

自定义view的基本流程 3

提到了滑动冲突怎么解决的

Android事件传递流程和OnTouchListener的关系

touch事件的传递机制

安卓的view事件分发机制 3

介绍一下view渲染流程

Android View绘制流程，当一个TextView的实例调用setText()方法后执行了什么

view绘制流程 2

view怎么确认位置与大小，测量模式

常用的viewgroup，与view区别，在事件处理过程中有什么区别

安卓点击事件的处理

lifecycle介绍

jetpack全家桶用过哪些

livedata有什么能力

双指缩放拖动大图

Android屏幕渲染机制

RecyclerView绘制步骤和复用机制

Recyclerview的复用机制

recyclerview的缓存机制（知道但不懂，我说做项目用list view出现了重复刷显示的问题，所以换了recyclerview解决

retrofit的具体实现，其中接口的作用，注解的作用

怎么在子线程中更新UI？

安卓存储方式

contentprovider介绍，能实现耗时操作吗

service介绍，生命周期与 contentprovider区别

介绍对安卓中intent的理解

service与activity通信

Kotlin有没有static 关键字？那创建静态函数怎么办

协程Flow有哪些应用场景？

内存泄漏的情况有哪些，讲一种检测方法

gradle打包流程

Android签名流程

对AMS的了解

content provider的作用。

sqlite的底层原理(不了解)

索引的原理及实现。

如何定位ANR和崩溃问题的原因。

compose和view写法的优缺点

使用recycleview碰到的问题

compose实现音乐播放栏固定

activity a -> b 的生命周期 （哪个阶段界面不可见）

fragment的构造函数初始化，navigate的跳转。两种方式理解

可以异步加载fragment吗，答案：可以。

serialVersionUID是否了解

handlerMessage什么时候会发生内存泄漏（要怎么预防）

图片有哪些格式，他们的区别

Fragment的replace,hide,add,show的区别

Retrofit中的Call对象如何转换成okhttp的call对象(这个题目是埋坑的)

Retrofit设计模式

加载so有几种方法

Handler工作机制
Looper如何识别Handler

# LIBRARY

OkHttp的原理

okhttp的请求机制

okhttp如何处理网络缓存的

okhttp拦截器

okhttp责任链设计模式

okhttp发送请求的拦截方式
okhttp的拦截器设计模式

retrofit怎么实现多线程 2

Retrofit的调用过程（我给你点提示，你自己思考一下）

viewmodel的实现原理 2

viewmodel怎么更新数据的

viewmodel实现原理

LeakCanary的使用和实现原理

安卓图片缓存，加载

协程实现原理

Glide的缓存机制 2

安卓glide中与生命周期的关系

glide的缓存加载机制

glide和OkHttp的任务调度是怎么实现的（比如同时发起很多请求）

okhttp、picasso等底层原理如缓存机制等（一个也没答上来，literally

# ACTIVITY

Activity遵循什么设计模式

Activity启动模式，allowReparent的特点和栈亲和性

从点击应用图标到进入应用，Android系统都做了哪些工作，期间涉及到的进程切换有哪些？

写个单例模式 -Activity启动模式

 Activity的启动流程,从Launcher到AMS——从AMS到ApplicationThread——从ApplicationThread到Activity

activity生命周期

onStart与onResume解释

activity四大启动模式

说下 Activity 的四种启动模式、应用场景 ？

——standard标准模式；singleTop 栈顶复用模式；singleTask 栈内复用模式；singleInstance 单实例模式

横竖屏切换的 Activity 生命周期变化？

fragment的生命周期

viewmodel设计模式，mvc,mvp,mvvm介绍

广播里怎么执行耗时操作

阻塞多久会出现ANR

Android intent如何传递数据
Android广播
谈谈Android性能优化

Activity的生命周期，从Activity A启动Activity B生命周期的变化

Activity生命周期，横竖屏切换的 Activity 生命周期变化？

SharedPreference 跨进程使用会怎么样？如何保证跨进程 使用安全？

activity，fragment 传值问题

activity 与 fragment 区别

Fragment 中 add 与 replace 的区别？

FragmentManager的Add和Replace区别

Activity因为内存不足销毁了如何恢复数据

点击锁屏后Activity会执行onPause和onStop吗

Activity的onDestory回调时机

Activity调用finish后是否立即onDestory

Activity调用finish后是否立即onDestory
Activity A 启动 Activity B,Activity A 的onDestory和Activity B 的onCreate执行顺序
Activity A 启动 Activity B,然后调用finish,Activity A 的onDestory和Activity B 的onCreate执行顺序
Activity的singleTop和singleTask的区别

binder原理

# KOTLIN

kotlin的let，apply，also有什么区别。
kotlin的inline，nonline关键字有什么作用。

异步调用有几种方式，从简单到复杂。

逆变与协变。

作用域函数（应用场景）

高阶函数（概念）

kotlin和java一块编译碰到啥问题

::funName 双冒号的写法的理解

泛型 out in 与Java泛型中的联系和区别

知道哪些高阶函数

协程的理解
协程相对于线程的区别

Kotlin 闭包
Kotlin 静态方法

链表逆序
Handler的postDelay原理
P2P网络

# THREAD

进程、线程、协程的联系与区别 2

Java创建线程的方式

线程并发会遇到哪些问题，怎么解决

线程的 wait 和 block 有什么区别？和 sleep 有什么区别？

sychronized和lock的区别

volatile详解、synchronized详解，两者区别

Java线程间通信， volatile详解、synchronized详解

线程的状态

线程池详解

线程池怎么做到线程复用

安卓中挂起函数怎么实现的

安卓中实现多线程的方法

线程池设计模式，怎么自己设计一个线程池

安卓里解决多线程冲突的方法

Android线程池设计原理

进程线程区别

进程通信方式有哪些
线程通信方式有哪些

安卓的线程通信

JAVAGC如何判断是否回收以及僵尸线程

双线程通过线程同步的方式打印12121212.......

为什么安卓用BINDER 有啥优点

安卓中进程间通信方式

线程安全的单例模式（双重检测），为什么要两次判断，volatile作用

多线程如何解决线程冲突？

Java加锁方式

死锁的必要条件 2

死锁怎么造成的

多线程会遇到什么问题

死锁的条件，手写一个死锁代码并运行出来

怎么避免死锁问题

可重入锁

对象、锁、对象监视器相互之间的关系？

乐观锁如何实现？缺点有哪些？

volatile 和 synchronized 有什么区别？

synchronized 和 lock 有什么区别？

synchronized 底层是如何实现加锁操作的？

ThreadLocal 是如何存储的？

ThreadLocal了解么，他有什么问题    

ThreadLocalMap中遇到冲突是如何处理的？

threadlocal，动态代理，这俩我说没用过不了解

synchronized 底层是如何实现的？

ReentrantLock 如何实现公平锁与非公平锁？

公平锁与非公平锁的释放有什么区别？

消费者与生产者模型

实现多线程的几种方式

乐观锁和悲观锁

 wait 和 sleep 的区别

 Android 线程间通信有哪几种方式

——1. 共享内存（变量）；2.文件，数据库；3.Handler；4.Java 里的 wait()，notify()，notifyAll()

 synchronized 原理 锁的升级 

 JVM内存模型 

生产者消费者模型，应用（线程池）

用过线程池吗，线程池如何实现

如何全局管理异步任务(不知道)

多线程下如何保证类的线程安全？

CopyOnWriteArrayList底层实现？如何实现线程安全？

线程池核心线程数数量的设计考虑因素

# MAP

### HashMap

HashMap的实现机制，怎么样HashMap线程安全

1.HashMap原理
 创建HashMap要放入1000个不同hashCode的键值对,初始最大值多少
HashMap如果Hash冲突了怎么解决？ 2

HashMap的扩容机制是什么样的？

concurrenthashmap详解

hashtable,hashmap与 concurrenthashmap详解

ConcurrentHashMap 是如何实现线程安全的？如果让你设计锁的数量你会怎么设计来提高效率？

HashMap的扩容机制，它线程安全吗？

哈希冲突解决方法，

hashmap插入数据的流程？

如何计算hashmap数据插入的位置？

如何解决哈希冲突？

有一千个键值对的数据，如何设计HashMap的初始容量大小？HashMap的实现原理 2

和HashTable区别，线程不安全的原因

哈希表解决冲突的方法

### ArrayList

线程安全的队列有哪些

ArrayList的扩容机制？

底层实现？深拷贝还是浅拷贝？

ArrayList的remove方法原理

ArrayList和LinkedList区别 2

面试官给了一段代码，问运行结果：代码是new了个空arraylist（string，integer）问这时候int i=arraylist.get（“key”）然后print会输出啥

如果是Integer i呢，会是啥（好家伙两个全答错）

# DESIGN MODEL

mvp是什么？2

MVVM与mvc、mvp架构的区别与联系

MVVM、MVC、MVP的区别与联系，各自优缺点

mvvm用到的设计模式

介绍一下中介者模式吧

问设计模式，要求挑一个深入分析，问你对这些模式的理解

除了中介者模式你还了解哪些设计模式

面向对象的原则有哪些

AOP了解吗

说说你对设计模式的理解，开发过程中主要用到了哪些设计模式？

介绍一下你在开发过程中使用到的设计模式

你都用过哪些设计模式？

Android中的ClassLoader

面向切面编程你知道么，和面向对象的区别和细节    

设计模式作用 

动态代理与静态代理 
