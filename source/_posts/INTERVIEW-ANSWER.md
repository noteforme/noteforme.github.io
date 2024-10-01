---
title: interview-answer
comments: true
date: 2018-03-12 12:44:01
tags: inter
categories: 
---

## [阿里面试题](https://www.jianshu.com/p/cf5092fa2694)

## 1.android事件分发机制，请详细说下整个流程

![img](https:////upload-images.jianshu.io/upload_images/2911038-5349d6ebb32372da?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

事件分发（面试）.png

## 2.android view绘制机制和加载过程，请详细说下整个流程

- 1.ViewRootImpl会调用performTraversals(),其内部会调用performMeasure()、performLayout、performDraw()。

- 2.performMeasure()会调用最外层的ViewGroup的measure()-->onMeasure(),ViewGroup的onMeasure()是抽象方法，但其提供了measureChildren()，这之中会遍历子View然后循环调用measureChild()这之中会用getChildMeasureSpec()+父View的MeasureSpec+子View的LayoutParam一起获取本View的MeasureSpec，然后调用子View的measure()到View的onMeasure()-->setMeasureDimension(getDefaultSize(),getDefaultSize()),getDefaultSize()默认返回measureSpec的测量数值，所以继承View进行自定义的wrap_content需要重写。

- 3.performLayout()会调用最外层的ViewGroup的layout(l,t,r,b),本View在其中使用setFrame()设置本View的四个顶点位置。在onLayout(抽象方法)中确定子View的位置，如LinearLayout会遍历子View，循环调用setChildFrame()-->子View.layout()。

- 4.performDraw()会调用最外层ViewGroup的draw():其中会先后调用background.draw()(绘制背景)、onDraw()(绘制自己)、dispatchDraw()(绘制子View)、onDrawScrollBars()(绘制装饰)。

- 5.MeasureSpec由2位SpecMode(UNSPECIFIED、EXACTLY(对应精确值和match_parent)、AT_MOST(对应warp_content))和30位SpecSize组成一个int,DecorView的MeasureSpec由窗口大小和其LayoutParams决定，其他View由父View的MeasureSpec和本View的LayoutParams决定。ViewGroup中有getChildMeasureSpec()来获取子View的MeasureSpec。

- 6.三种方式获取measure()后的宽高：
  
  - 1.Activity#onWindowFocusChange()中调用获取
  - 2.view.post(Runnable)将获取的代码投递到消息队列的尾部。
  - 3.ViewTreeObservable.
  
  自定义 View 的绘制顺序

## 3.android四大组件的加载过程，请详细介绍下

- 1.[android四大组件的加载过程](https://www.jianshu.com/p/f499afd8d0ab):这是我总结的一篇博客

## 4.Activity的启动模式

- 1.standard:默认标准模式，每启动一个都会创建一个实例，
- 2.singleTop：栈顶复用，如果在栈顶就调用onNewIntent复用，从onResume()开始
- 3.singleTask：栈内复用，本栈内只要用该类型Activity就会将其顶部的activity出栈
- 4.singleInstance：单例模式，除了3中特性，系统会单独给该Activity创建一个栈，

## 5.A、B、C、D分别是四种Activity的启动模式，那么A->B->C->D->A->B->C->D分别启动，最后的activity栈是怎么样的

- 1.这个题目需要深入了解activity的启动模式
- 2.最后的答案是：两个栈，前台栈是只有D，后台栈从底至上是A、B、C

## 6.Activity缓存方法

- 1.配置改变导致Activity被杀死，横屏变竖屏：在onStop之前会调用onSaveInstanceState()保存数据在重建Activity之后，会在onStart()之后调用onRestoreInstanceState(),并把保存下来的Bundle传给onCreate()和它会默认重建Activity当前的视图，我们可以在onCreate()中，回复自己的数据。
- 2.内存不足杀掉Activity，优先级分别是：前台可见，可见非前台，后台。

## 7.Service的生命周期，两种启动方法，有什么区别

- 1.context.startService() ->onCreate()- >onStart()->Service running-->(如果调用context.stopService() )->onDestroy() ->Service shut down
  - 1.如果Service还没有运行，则调用onCreate()然后调用onStart()；
  - 2.如果Service已经运行，则只调用onStart()，所以一个Service的onStart方法可能会重复调用多次。
  - 3.调用stopService的时候直接onDestroy，
  - 4.如果是调用者自己直接退出而没有调用stopService的话，Service会一直在后台运行。该Service的调用者再启动起来后可以通过stopService关闭Service。
- 2.context.bindService()->onCreate()->onBind()->Service running-->onUnbind() -> onDestroy() ->Service stop
  - 1.onBind将返回给客户端一个IBind接口实例，IBind允许客户端回调服务的方法，比如得到Service运行的状态或其他操作。
  - 2.这个时候会把调用者和Service绑定在一起，Context退出了,Service就会调用onUnbind->onDestroy相应退出。
  - 3.所以调用bindService的生命周期为：onCreate --> onBind(只一次，不可多次绑定) --> onUnbind --> onDestory。

## 8.怎么保证service不被杀死

- 1.提升service优先级
- 2.提升service进程优先级
- 3.onDestroy方法里重启service

## 9.静态的Broadcast 和动态的有什么区别

- 1.动态的比静态的安全
- 2.静态在app启动的时候就初始化了 动态使用代码初始化
- 3.静态需要配置 动态不需要
- 4.生存期，静态广播的生存期可以比动态广播的长很多
- 5.优先级动态广播的优先级比静态广播高

## 10.Intent可以传递哪些数据类型

- 1.Serializable
- 2.charsequence: 主要用来传递String，char等
- 3.parcelable
- 4.Bundle

## 11.Json有什么优劣势、解析的原理

- 1.JSON的速度要远远快于XML
- 2.JSON相对于XML来讲，数据的体积小
- 3.JSON对数据的描述性比XML较差
- 4.解析的基本原理是：词法分析

## 12.一个语言的编译过程

- 1.词法分析：将一串文本按规则分割成最小的结构，关键字、标识符、运算符、界符和常量等。一般实现方法是自动机和正则表达式
- 2.语法分析：将一系列单词组合成语法树。一般实现方法有自顶向下和自底向上
- 3.语义分析：对结构上正确的源程序进行上下文有关性质的审查
- 4.目标代码生成
- 5.代码优化：优化生成的目标代码，

## 13.动画有哪几类，各有什么特点

- 1.动画的基本原理：其实就是利用插值器和估值器，来计算出各个时刻View的属性，然后通过改变View的属性来，实现View的动画效果。
- 2.View动画:只是影像变化，view的实际位置还在原来的地方。
- 3.帧动画是在xml中定义好一系列图片之后，使用AnimationDrawable来播放的动画。
- 4.View的属性动画：
  - 1.插值器：作用是根据时间的流逝的百分比来计算属性改变的百分比
  - 2.估值器：在1的基础上由这个东西来计算出属性到底变化了多少数值的类

## 14.Handler、Looper消息队列模型，各部分的作用

- 1.MessageQueue：读取会自动删除消息，单链表维护，在插入和删除上有优势。在其next()中会无限循环，不断判断是否有消息，有就返回这条消息并移除。
- 2.Looper：Looper创建的时候会创建一个MessageQueue，调用loop()方法的时候消息循环开始，loop()也是一个死循环，会不断调用messageQueue的next()，当有消息就处理，否则阻塞在messageQueue的next()中。当Looper的quit()被调用的时候会调用messageQueue的quit(),此时next()会返回null，然后loop()方法也跟着退出。
- 3.Handler：在主线程构造一个Handler，然后在其他线程调用sendMessage(),此时主线程的MessageQueue中会插入一条message，然后被Looper使用。
- 4.系统的主线程在ActivityThread的main()为入口开启主线程，其中定义了内部类Activity.H定义了一系列消息类型，包含四大组件的启动停止。
- 5.MessageQueue和Looper是一对一关系，Handler和Looper是多对一

## 15.怎样退出终止App

- 1.自己设置一个Activity的栈，然后一个个finish()

## 16.Android IPC:Binder原理

- 1.在Activity和Service进行通讯的时候，用到了Binder。
  - 1.当属于同个进程我们可以继承Binder然后在Activity中对Service进行操作
  - 2.当不属于同个进程，那么要用到AIDL让系统给我们创建一个Binder，然后在Activity中对远端的Service进行操作。
- 2.系统给我们生成的Binder：
  - 1.Stub类中有:接口方法的id，有该Binder的标识，有asInterface(IBinder)(让我们在Activity中获取实现了Binder的接口，接口的实现在Service里，同进程时候返回Stub否则返回Proxy)，有onTransact()这个方法是在不同进程的时候让Proxy在Activity进行远端调用实现Activity操作Service
  - 2.Proxy类是代理，在Activity端，其中有:IBinder mRemote(这就是远端的Binder)，两个接口的实现方法不过是代理最终还是要在远端的onTransact()中进行实际操作。
- 3.哪一端的Binder是副本，该端就可以被另一端进行操作，因为Binder本体在定义的时候可以操作本端的东西。所以可以在Activity端传入本端的Binder，让Service端对其进行操作称为Listener，可以用RemoteCallbackList这个容器来装Listener，防止Listener因为经历过序列化而产生的问题。
- 4.当Activity端向远端进行调用的时候，当前线程会挂起，当方法处理完毕才会唤醒。
- 5.如果一个AIDL就用一个Service太奢侈，所以可以使用Binder池的方式，建立一个AIDL其中的方法是返回IBinder，然后根据方法中传入的参数返回具体的AIDL。
- 6.IPC的方式有：Bundle（在Intent启动的时候传入，不过是一次性的），文件共享(对于SharedPreference是特例，因为其在内存中会有缓存)，使用Messenger(其底层用的也是AIDL，同理要操作哪端，就在哪端定义Messenger)，AIDL，ContentProvider(在本进程中继承实现一个ContentProvider，在增删改查方法中调用本进程的SQLite，在其他进程中查询)，Socket

## 17.描述一次跨进程通讯

- 1.client、proxy、serviceManager、BinderDriver、impl、service
- 2.client发起一个请求service信息的Binder请求到BinderDriver中，serviceManager发现BinderDiriver中有自己的请求 然后将clinet请求的service的数据返回给client这样完成了一次Binder通讯
- 3.clinet获取的service信息就是该service的proxy，此时调用proxy的方法，proxy将请求发送到BinderDriver中，此时service的 Binder线程池循环发现有自己的请求，然后用impl就处理这个请求最后返回，这样完成了第二次Binder通讯
  4.中间client可挂起，也可以不挂起，有一个关键字oneway可以解决这个

## 18.android重要术语解释

- 1.ActivityManagerServices，简称AMS，服务端对象，负责系统中所有Activity的生命周期
- 2.ActivityThread，App的真正入口。当开启App之后，会调用main()开始运行，开启消息循环队列，这就是传说中的UI线程或者叫主线程。与ActivityManagerServices配合，一起完成Activity的管理工作
- 3.ApplicationThread，用来实现ActivityManagerService与ActivityThread之间的交互。在ActivityManagerService需要管理相关Application中的Activity的生命周期时，通过ApplicationThread的代理对象与ActivityThread通讯。
- 4.ApplicationThreadProxy，是ApplicationThread在服务器端的代理，负责和客户端的ApplicationThread通讯。AMS就是通过该代理与ActivityThread进行通信的。
- 5.Instrumentation，每一个应用程序只有一个Instrumentation对象，每个Activity内都有一个对该对象的引用。Instrumentation可以理解为应用进程的管家，ActivityThread要创建或暂停某个Activity时，都需要通过Instrumentation来进行具体的操作。
- 6.ActivityStack，Activity在AMS的栈管理，用来记录已经启动的Activity的先后关系，状态信息等。通过ActivityStack决定是否需要启动新的进程。
- 7.ActivityRecord，ActivityStack的管理对象，每个Activity在AMS对应一个ActivityRecord，来记录Activity的状态以及其他的管理信息。其实就是服务器端的Activity对象的映像。
- 8.TaskRecord，AMS抽象出来的一个“任务”的概念，是记录ActivityRecord的栈，一个“Task”包含若干个ActivityRecord。AMS用TaskRecord确保Activity启动和退出的顺序。如果你清楚Activity的4种launchMode，那么对这个概念应该不陌生。

## 19.理解Window和WindowManager

- 1.Window用于显示View和接收各种事件，Window有三种类型：应用Window(每个Activity对应一个Window)、子Window(不能单独存在，附属于特定Window)、系统window(Toast和状态栏)
- 2.Window分层级，应用Window在1-99、子Window在1000-1999、系统Window在2000-2999.WindowManager提供了增删改View三个功能。
- 3.Window是个抽象概念：每一个Window对应着一个View和ViewRootImpl，Window通过ViewRootImpl来和View建立联系，View是Window存在的实体，只能通过WindowManager来访问Window。
- 4.WindowManager的实现是WindowManagerImpl其再委托给WindowManagerGlobal来对Window进行操作，其中有四个List分别储存对应的View、ViewRootImpl、WindowManger.LayoutParams和正在被删除的View
- 5.Window的实体是存在于远端的WindowMangerService中，所以增删改Window在本端是修改上面的几个List然后通过ViewRootImpl重绘View，通过WindowSession(每个应用一个)在远端修改Window。
- 6.Activity创建Window：Activity会在attach()中创建Window并设置其回调(onAttachedToWindow()、dispatchTouchEvent()),Activity的Window是由Policy类创建PhoneWindow实现的。然后通过Activity#setContentView()调用PhoneWindow的setContentView。

## 20.Bitmap的处理

- 1.当使用ImageView的时候，可能图片的像素大于ImageView，此时就可以通过BitmapFactory.Option来对图片进行压缩，inSampleSize表示缩小2^(inSampleSize-1)倍。
- 2.BitMap的缓存：
  - 1.使用LruCache进行内存缓存。
  - 2.使用DiskLruCache进行硬盘缓存。
  - 3.实现一个ImageLoader的流程：同步异步加载、图片压缩、内存硬盘缓存、网络拉取
    - 1.同步加载只创建一个线程然后按照顺序进行图片加载
    - 2.异步加载使用线程池，让存在的加载任务都处于不同线程
    - 3.为了不开启过多的异步任务，只在列表静止的时候开启图片加载

## 21.如何实现一个网络框架

- 1.缓存队列,以url为key缓存内容可以参考Bitmap的处理方式，这里单独开启一个线程。
- 2.网络请求队列，使用线程池进行请求。
- 3.提供各种不同类型的返回值的解析如String，Json，图片等等。

## 22.ClassLoader的基础知识

- 1.双亲委托：一个ClassLoader类负责加载这个类所涉及的所有类，在加载的时候会判断该类是否已经被加载过，然后会递归去他父ClassLoader中找。
- 2.可以动态加载Jar通过URLClassLoader
- 3.ClassLoader 隔离问题 JVM识别一个类是由：ClassLoader id+PackageName+ClassName。
- 4.加载不同Jar包中的公共类：
  - 1.让父ClassLoader加载公共的Jar，子ClassLoader加载包含公共Jar的Jar，此时子ClassLoader在加载公共Jar的时候会先去父ClassLoader中找。(只适用Java)
  - 2.重写加载包含公共Jar的Jar的ClassLoader，在loadClass中找到已经加载过公共Jar的ClassLoader，也就是把父ClassLoader替换掉。(只适用Java)
  - 3.在生成包含公共Jar的Jar时候把公共Jar去掉。

## 25.线程同步的问题，常用的线程同步

- 1.sycn：保证了原子性、可见性、有序性
- 2.锁：保证了原子性、可见性、有序性
  - 1.自旋锁:可以使线程在没有取得锁的时候，不被挂起，而转去执行一个空循环。
    - 1.优点:线程被挂起的几率减少，线程执行的连贯性加强。用于对于锁竞争不是很激烈，锁占用时间很短的并发线程。
    - 2.缺点:过多浪费CPU时间，有一个线程连续两次试图获得自旋锁引起死锁
  - 2.阻塞锁:没得到锁的线程等待或者挂起，Sycn、Lock
  - 3.可重入锁:一个线程可多次获取该锁，Sycn、Lock
  - 4.悲观锁:每次去拿数据的时候都认为别人会修改，所以会阻塞全部其他线程 Sycn、Lock
  - 5.乐观锁:每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。cas
  - 6.显示锁和内置锁:显示锁用Lock来定义、内置锁用synchronized。
  - 7.读-写锁:为了提高性能，Java提供了读
- 3.volatile
  - 1.只能保证可见性，不能保证原子性
  - 2.自增操作有三步，此时多线程写会出现问题
- 4.cas
  - 1.操作:内存值V、旧的预期值A、要修改的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做并返回false。
  - 2.解释:本地副本为A，共享内存为V，线程A要把V修改成B。某个时刻线程A要把V修改成B，如果A和V不同那么就表示有其他线程在修改V，此时就表示修改失败，否则表示没有其他线程修改，那么把V改成B。
  - 3.局限:如果V被修改成V1然后又被改成V，此时cas识别不出变化，还是认为没有其他线程在修改V，此时就会有问题
  - 4.局限解决:将V带上版本。
- 5.线程不安全到底是怎么回事：
  - 1.一个线程写，多个线程读的时候，会造成写了一半就去读
  - 2.多线程写，会造成脏数据

## 26.Asynctask和线程池，GC相关（怎么判断哪些内存该GC，GC算法）

- 1.Asynctask：异步任务类，单线程线程池+Handler
- 2.线程池：
  - 1.ThreadPoolExecutor：通过Executors可以构造单线程池、固定数目线程池、不固定数目线程池。
  - 2.ScheduledThreadPoolExecutor：可以延时调用线程或者延时重复调度线程。
- 3.GC相关：重要
  - 1.搜索算法：
    - 1.引用计数
    - 2.图搜索，可达性分析
  - 2.回收算法：
    - 1.标记清除复制：用于青年代
    - 2.标记整理：用于老年代
  - 3.堆分区：
    - 1.青年区eden 80%、survivor1 10%、survivor2 10%
    - 2.老年区
  - 4.虚拟机栈分区：
    - 1.局部变量表
    - 2.操作数栈
    - 3.动态链接
    - 4.方法返回地址
  - 5.GC Roots:
    - 1.虚拟机栈(栈桢中的本地变量表)中的引用的对象
    - 2.方法区中的类静态属性引用的对象
    - 3.方法区中的常量引用的对象
    - 4.本地方法栈中JNI的引用的对象

## 28.数据库性能优化：索引和事务，需要找本专门的书大概了解一下

## 29.13.APK打包流程和其内容

- 1.流程
  - 1.aapt生成R文件
    - 2.aidl生成java文件
    - 3.将全部java文件编译成class文件
    - 4.将全部class文件和第三方包合并成dex文件
    - 5.将资源、so文件、dex文件整合成apk
    - 6.apk签名
    - 7.apk字节对齐
- 2.内容：so、dex、asset、资源文件

## 

## 31.java类加载过程：

- 1.加载时机：创建实例、访问静态变量或方法、反射、加载子类之前
- 2.验证：验证文件格式、元数据、字节码、符号引用的正确性
- 3.加载：根据全类名获取文件字节流、将字节流转化为静态储存结构放入方法区、生成class对象
- 4.准备：在堆上为静态变量划分内存
- 5.解析：将常量池中的符号引用转换为直接引用
- 6.初始化：初始化静态变量
- 7.书籍推荐：**深入理解java虚拟机**，博客推荐：[Java/Android阿里面试JVM部分理解](https://www.jianshu.com/p/bc6d1770d92c)

## 32.retrofit的了解

- 1.动态代理创建一个接口的代理类
- 2.通过反射解析每个接口的注解、入参构造http请求
- 3.获取到返回的http请求，使用Adapter解析成需要的返回值。

## 33.bundle的数据结构，如何存储

- 1.键值对储存
- 2.传递的数据可以是boolean、byte、int、long、float、double、string等基本类型或它们对应的数组，也可以是对象或对象数组。
- 3.当Bundle传递的是对象或对象数组时，必须实现Serializable 或Parcelable接口

## 34.listview内点击buttom并移动的事件流完整拦截过程：

- 1.点下按钮的时候：
  - 1.产生了一个down事件，activity-->phoneWindow-->ViewGroup-->ListView-->botton,中间如果有重写了拦截方法，则事件被该view拦截可能消耗。
  - 2.没拦截，事件到达了button，这个过程中建立了一条事件传递的view链表
  - 3.到button的dispatch方法-->onTouch-->view是否可用-->Touch代理
- 2.移动点击按钮的时候:
  - 1.产生move事件，listView中会对move事件做拦截
  - 2.此时listView会将该滑动事件消费掉
  - 3.后续的滑动事件都会被listView消费掉
- 3.手指抬起来时候：前面建立了一个view链表，listView的父view在获取事件的时候，会直接取链表中的listView让其进行事件消耗。

## 35.service的意义：不需要界面，在后台执行的程序

## 36.android的IPC通信方式，线程（进程间）通信机制有哪些

- 1.ipc通信方式：binder、contentprovider、socket
- 2.操作系统进程通讯方式：共享内存、socket、管道

## 37.操作系统进程和线程的区别

- 1.简而言之,一个程序至少有一个进程,一个进程至少有一个线程.
- 2.线程的划分尺度小于进程，使得多线程程序的并发性高。
- 3.另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。
- 4.多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配

## 38.HashMap的实现过程：Capacity就是buckets的数目，Load factor就是buckets填满程度的最大比例。如果对迭代性能要求很高的话不要把capacity设置过大，也不要把load factor设置过小。

- 1.简单来说HashMap就是一个会自动扩容的**数组链表**
- 2.put过程
  - 1.对key的hashCode()做hash，然后再计算index;
  - 2.如果没碰撞直接放到bucket里；
  - 3.如果碰撞了，以链表的形式存在buckets后；
  - 4.如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD)，就把链表转换成红黑树；
  - 5.如果节点已经存在就替换old value(保证key的唯一性)
  - 6.如果bucket满了(超过load factor*current capacity)，就要resize。
- 3.resize：当put时，如果发现目前的bucket占用程度已经超过了Load Factor所希望的比例，那么就会发生resize。在resize的过程，简单的说就是把bucket扩充为2倍，之后重新计算index，把节点再放到新的bucket中
- 4.get过程
  - 1.根据key的hash算出数组下表
  - 2.使用equals遍历链表进行比较

## 39.mvc、mvp、mvvm：

- 1.mvc:数据、View、Activity，View将操作反馈给Activity，Activitiy去获取数据，数据通过观察者模式刷新给View。循环依赖
  - 1.Activity重，很难单元测试
  - 2.View和Model耦合严重
- 2.mvp:数据、View、Presenter，View将操作给Presenter，Presenter去获取数据，数据获取好了返回给Presenter，Presenter去刷新View。PV，PM双向依赖
  - 1.接口爆炸
  - 2.Presenter很重
- 3.mvvm:数据、View、ViewModel，View将操作给ViewModel，ViewModel去获取数据，数据和界面绑定了，数据更新界面更新。
  - 1.viewModel的业务逻辑可以单独拿来测试
  - 2.一个view 对应一个 viewModel 业务逻辑可以分离，不会出现全能类
  - 3.数据和界面绑定了，不用写垃圾代码，但是复用起来不舒服

## 40.java的线程如何实现

- 1.Thread继承
- 2.Runnale
- 3.Future
- 4.线程池

## 41.ArrayList 如何删除重复的元素或者指定的元素；

- 1.删除重复：Set
- 2.删除指定：迭代器

## 42.如何设计在 UDP 上层保证 UDP 的可靠性传输；

- 1.简单来讲，要使用UDP来构建可靠的面向连接的数据传输，就要实现类似于TCP协议的超时重传，有序接受，应答确认，滑动窗口流量控制等机制,等于说要在传输层的上一层（或者直接在应用层）实现TCP协议的可靠数据传输机制。
- 2.比如使用UDP数据包+序列号，UDP数据包+时间戳等方法，在服务器端进行应答确认机制，这样就会保证不可靠的UDP协议进行可靠的数据传输。
- 3.基于udp的可靠传输协议有：RUDP、RTP、UDT

## 43.Java 中内部类为什么可以访问外部类

- 1.因为内部类创建的时候，需要外部类的对象，在内部类对象创建的时候会把外部类的引用传递进去

## 44.设计移动端的联系人存储与查询的功能，要求快速搜索联系人，可以用到哪些数据结构？数据库索引，平衡二叉树(B树、红黑树)

## 45.红黑树特点

- 1.root节点和叶子节点是黑色
- 2.红色节点后必须为黑色节点
- 3.从root到叶子每条路径的黑节点数量相同

## 46.linux异步和同步i/o:

- 1.同步：对于client，client一直等待，但是client不挂起：主线程调用
- 2.异步：对于client，client发起请求，service好了再回调client：其他线程调用，调用完成之后进行回调
- 3.阻塞：对于service，在准备io的时候会将service端挂起，直至准备完成然后唤醒service：bio
- 3.非阻塞：对于service，在准备io的时候不会将service端挂起，而是service一直去轮询判断io是否准备完成，准备完成了就进行操作：nio、linux的select、poll、epoll
- 4.多路复用io：非阻塞io的一种优化，java nio，用一个线程去轮询多个 io端口是否可用，如果一个可用就通知对应的io请求，这使用一个线程轮询可以大大增强性能。
  - 1.我可以采用 多线程+ 阻塞IO 达到类似的效果，但是由于在多线程 + 阻塞IO 中，每个socket对应一个线程，这样会造成很大的资源占用。
  - 2.而在多路复用IO中，轮询每个socket状态是内核在进行的，这个效率要比用户线程要高的多。
- 5.异步io：aio，用户线程完全不感知io的进行，所有操作都交给内核，io完成之后内核通知用户线程。
  - 1.这种io才是异步的，2、3、4都是同步io，因为内核进行数据拷贝的过程都会让用户线程阻塞。
  - 2.异步IO是需要操作系统的底层支持，也就是内核支持，Java 7中，提供了Asynchronous IO

## 47.ConcurrentHashMap内部实现，HashTable的实现被废弃的原因:

- 1.HashTable容器在竞争激烈的并发环境下表现出效率低下的原因，是因为所有访问HashTable的线程都必须竞争同一把锁，那假如容器里有多把锁，每一把锁用于锁容器其中一部分数据，那么当多线程访问容器里不同数据段的数据时，线程间就不会存在锁竞争，从而可以有效的提高并发访问效率，这就是ConcurrentHashMap所使用的锁分段技术，首先将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。
- 2.ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment数组，Segment的结构和HashMap类似，是一种数组和链表结构， 一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元素，每个Segment守护者一个HashEntry数组里的元素,当对HashEntry数组的数据进行修改时，必须首先获得它对应的Segment锁。

## 48.HandlerThread是什么

- 1.MessageQueue + Looper + Handler

## 49.IntentService是什么

- 1.含有HandlerThread的Service，可以多次startService()来多次在子线程中进行 onHandlerIntent()的调用。

## 50.class和dex

- 1.dvm执行的是dex格式文件，jvm执行的是class文件，android程序编译完之后生产class文件。然后dex工具会把class文件处理成dex文件，然后把资源文件和.dex文件等打包成apk文件。
- 2.dvm是基于寄存器的虚拟机，而jvm执行是基于虚拟栈的虚拟机。寄存器存取速度比栈快的多，dvm可以根据硬件实现最大的优化，比较适合移动设备。
- 3.class文件存在很多的冗余信息，dex工具会去除冗余信息，并把所有的class文件整合到dex文件中。减少了I/O操作，提高了类的查找速度

## 51.内存泄漏

- 1.其他线程持有一个Listener，Listener操作activity。那么在线程么有完毕的时候，activity关闭了，原本是要被回收的但是，不能被回收。
- 2.例如Handler导致的内存泄漏，Handler就相当于Listener。
- 3.在activity关闭的时候注意停止线程，或者将Listener的注册取消
- 3.使用弱引用，这样即使Listener持有了activity，在GC的时候还是会被回收
- 4.工具:LeakCanary

## 52.过度绘制、卡顿优化:

- 1.过度绘制：
  - 1.移除Window默认的Background：getWidow.setBackgroundDrawable(null);
  - 2.移除XML布局文件中非必需的Background
  - 3.减少布局嵌套(扁平化的一个体现，减少View数的深度，也就减少了View树的遍历时间，渲染的时候，前后期的工作，总是按View树结点来)
  - 4.在引入布局文件里面，最外层可以用merge替代LinearLayout,RelativeLayout，这样把子UI元素直接衔接在include位置
  - 5.工具：HierarchyViewer 查看视图层级
- 2.卡顿优化：16ms数据更新

## 53.apk瘦身:

- 1.classes.dex：通过代码混淆，删掉不必要的jar包和代码实现该文件的优化
- 2.资源文件：通过Lint工具扫描代码中没有使用到的静态资源
- 3.图片资源：使用tinypng和webP，下面详细介绍图片资源优化的方案,矢量图
- 4.SO文件将不用的去掉，目前主流app一般只放一个arm的so包

## 54.ANR的形成，各个组件上出现ARN的时间限制是多少

- 1.只要是主线程耗时的操作就会ARN  如io
- 2.broadcast超时时间为10秒  按键无响应的超时时间为5秒 前台service无响应的超时时间为20秒，后台service为200秒

## 55.Serializable和Parcelable 的区别

- 1.P 消耗内存小
- 2.网络传输用S  程序内使用P
- 3.S将数据持久化方便
- 4.S使用了反射 容易触发垃圾回收 比较慢

## 56.Sharedpreferences源码简述

- 1.储存于硬盘上的xml键值对，数据多了会有性能问题
- 2.ContextImpl记录着SharedPreferences的重要数据，文件路径和实例的键值对
- 3.在xml文件全部内加载到内存中之前，读取操作是阻塞的，在xml文件全部内加载到内存中之后，是直接读取内存中的数据
- 4.apply因为是异步的没有返回值, commit是同步的有返回值能知道修改是否提交成功
- 5.多并发的提交commit时，需等待正在处理的commit数据更新到磁盘文件后才会继续往下执行，从而降低效率; 而apply只是原子更新到内存，后调用apply函数会直接覆盖前面内存数据，从一定程度上提高很多效率。 3.edit()每次都是创建新的EditorImpl对象.
- 6.博客推荐：**[全面剖析SharedPreferences](https://www.jianshu.com/p/102f25cf64e3)**

## 57.操作系统如何管理内存的：

- 1.使用寄存器进行将进程地址和物理内存进行映射
- 2.虚拟内存进行内存映射到硬盘上增大内存
- 3.虚拟内存是进行内存分页管理
- 4.页表实现分页，就是 页+地址偏移。
- 5.如果程序的内存在硬盘上，那么就需要用页置换算法来将其调入内存中：先进先出、最近未使用最少等等
- 6.博客推荐：**[现代操作系统部分章节笔记](https://www.jianshu.com/p/aecff59430fa)**

## 58.浏览器输入地址到返回结果发生了什么

- 1.DNS解析
- 2.TCP连接
- 3.发送HTTP请求
- 4.服务器处理请求并返回HTTP报文
- 5.浏览器解析渲染页面
- 6.连接结束

## 59.java泛型类型擦除发生在什么时候，通配符有什么需要注意的。

- 1.发生在编译的时候
- 2.PECS，extends善于提供精确的对象 A是B的子集，Super善于插入精确的对象 A是B的超集
- 3.博客推荐：**[Effective Java笔记（不含反序列化、并发、注解和枚举）](https://www.jianshu.com/p/4e4751b5bbbb)**、**[android阿里面试java基础锦集](https://www.jianshu.com/p/6006a3284f55)**

## 60.activity的生命周期

- 1.a启动b，后退键再到a的生命周期流程：a.create-->a.start-->a.resume-->a.pause-->b.create-->b.start-->b.resume-->b界面绘制-->a.stop-->b.pause-->b.stop-->b.destroy-->a.restart-->a.start-->a.resume
- 2.意外销毁会调用saveInstance，重新恢复的时候回调用restoreInstance。储存数据的时候使用了委托机制，从activity-->window-->viewGroup-->view 会递归调用save来保持本view的数据，restore则是递归恢复本view数据。我们可以在里面做一些自己需要的数据操作。

## 61.面试常考的算法

- 1.快排、堆排序为首的各种排序算法
- 2.链表的各种操作：判断成环、判断相交、合并链表、倒数K个节点、寻找成环节点
- 3.二叉树、红黑树、B树定义以及时间复杂度计算方式
- 4.动态规划、贪心算法、简单的图论
- 5.推荐书籍：**算法导论**，将图论之前的例子写一遍

## 62.Launcher进程启动另外一个进程的过程：[启动一个app](http://www.cnblogs.com/tiantianbyconan/p/5017056.html)

## 63.开源框架源码

- 1.Fresco
  - 1.mvc框架：
    - 1.Controller控制数据显示在Hierarchy中的Drawable的显隐
    - 2.ImagePipeline在Controller中负责进行数据获取，返回的数据是CloseableImage
    - 3.Drawee把除了初始化之外的操作全部交给Holder去做，Holder持有Controller和Hierarchy
  - 2.Drawable层次以及绘制：
    - 1.如果要绘制一次Drawable就调用invalidateSelf()来触发onDraw()
    - 2.Drawable分为：容器类(保存一些Drawable)、自我绘制类(进度条)、图形变换类(scale、rotate、矩阵变换)、动画类(内部不断刷新，进行webp和gif的帧绘制)
    - 3.ImagePipeline返回的CloseableImage是由一个个DrawableFactory解析成Drawable的
    - 4.webp和gif动画是由jni代码解析的，然后其他静态图片是根据不同的android平台使用BitmapFactory来解析的
  - 3.职责链模式：producer不做操作标n，表示只是提供一个consumer。获取图片--》解码图片缓存Producer--》后台线程Producer--》client图片处理producer(n)--》解码producer(n)--》旋转或剪裁producer(n)--》编码图片内存缓存producer--》读硬盘缓存producer--》写硬盘缓存producer(n)--》网络producer提供CloseableImage《--解码图片缓存consumer《--client图片处理consumer《--解码consumer《--旋转或剪裁consumer《--编码图片内存缓存consumer《--写硬盘缓存consumer《--图片数据
  - 4.内存缓存：
    - 1.一个CountingLruMap保存已经没有被引用的缓存条目，一个CountingLruMap保存所有的条目包括没有引用的条目。每当缓存策略改变和一定时间缓存配置的更新的时候，就会将 待销毁条目Map中的条目一个个移除，直到缓存大小符合配置。
    - 2.这里的引用计数是用Fresco组件实现的引用计数器。
    - 3.缓存有一个代理类，用来追踪缓存的存取。
    - 4.CountingLruMap是使用LinkedHashMap来储存数据的。
  - 5.硬盘缓存：
    - 1.DefaultDiskStorage使用Lru策略。
    - 2.为了不让所有的文件集中在一个文件中，创建很多命名不同的文件夹，然后使用hash算法把缓存文件分散
    - 3.DiskStorageCache封装了DefaultDiskStorage，不仅进行缓存存取追踪，并且其在内存里面维持着一个 <key,value> 的键值对，因为文件修改频繁，所有只是定时刷新，因此如果在内存中找不到，还要去硬盘中找一次。
    - 4.删除硬盘的缓存只出现在硬盘数据大小超限的时候，此时同时也会删除缓存中的key，所以不会出现内存中有key，但是硬盘上没有的情况。
    - 5.在插入硬盘数据的时候，采用的是插入器的形式。返回一个Inserter，在Inserter.writeData()中传入一个CallBack(里面封装了客户端插入数据的逻辑和文件引用)，让内部实现调用CallBack的逻辑来插入文件数据，前面写的文件后缀是.temp,只有调用commit()之后才会修改后缀，让文件对客户端可见。
    - 6.使用了java提供的FileTreeVisitor来遍历文件
  - 6.对象池：
    - 1.使用数组来存储一个桶，桶内部是一个Queue。数组下标是数据申请内存的byte大小，桶内部的Queue存的是内存块的。所以数组使用的是稀疏数组
    - 2.申请内存的方式有两种 1.java堆上开辟的内存 2.ashme 的本地内存中开辟的内存
  - 7.设计模式：Builder、职责链、观察者、代理、组合、享元、适配器、装饰者、策略、生产者消费者、提供者
  - 8.自定义计数引用：类似c++智能指针
    - 1.使用一个静态IdentityHashMap <储存需要被计数引用的对象,其被引用的次数>
    - 2.用SharedReference分装需要被计数引用的对象，提供一个销毁资源的销毁器，提供一个静态工厂方法来复制自己，复制一个引用计数加一。提供一个方法销毁自己，表示自己需要变成无人引用的对象了，此时引用计数减一。
    - 3.引用计数归零，销毁器将销毁资源，如bitmap的recycle或者是jni内存调用jni方法归还内存。
  - 9.博客推荐：**[Android Fresco源码文档翻译](https://www.jianshu.com/p/dbe01f9994d0)**、**[从零开始撸一个Fresco之硬盘缓存](https://www.jianshu.com/p/ab2124764438)**、**[从零开始撸一个Fresco之gif和Webp动画](https://www.jianshu.com/p/36663090b140)**、**[从零开始撸一个Fresco之内存缓存](https://www.jianshu.com/p/ba0de15ce667)**、**[从零开始撸一个Fresco之总结](https://www.jianshu.com/p/2dff47ae7666)**
- 2.oKhttp：
  - 1.同步和异步：
    - 1.异步使用了Dispatcher来将存储在 Deque 中的请求分派给线程池中各个线程执行。
    - 2.当任务执行完成后，无论是否有异常，finally代码段总会被执行，也就是会调用Dispatcher的finished函数，它将正在运行的任务Call从队列runningAsyncCalls中移除后，主动的把缓存队列向前走了一步。
  - 2.连接池：
    - 1.一个Connection封装了一个socket，ConnectionPool中储存s着所有的Connection，StreamAllocation是引用计数的一个单位
    - 2.当一个请求获取一个Connection的时候要传入一个StreamAllocation，Connection中存着一个弱引用的StreamAllocation列表，每当上层应用引用一次Connection，StreamAllocation就会加一个。反之如果上层应用不使用了，就会删除一个。
    - 3.ConnectionPool中会有一个后台任务定时清理StreamAllocation列表为空的Connection。5分钟时间，维持5个socket
  - 3.选择路线与建立连接
    - 1.选择路线有两种方式：
      - 1.无代理，那么在本地使用DNS查找到ip，注意结果是数组，即一个域名有多个IP，这就是自动重连的来源
      - 2.有代理HTTP：设置socket的ip为代理地址的ip，设置socket的端口为代理地址的端口
      - 3.代理好处：HTTP代理会帮你在远程服务器进行DNS查询，可以减少DNS劫持。
    - 2.建立连接
      - 1.连接池中已经存在连接，就从中取出(get)RealConnection，如果没有命中就进入下一步
      - 2.根据选择的路线(Route)，调用Platform.get().connectSocket选择当前平台Runtime下最好的socket库进行握手
      - 3.将建立成功的RealConnection放入(put)连接池缓存
      - 4.如果存在TLS，就根据SSL版本与证书进行安全握手
      - 5.构造HttpStream并维护刚刚的socket连接，管道建立完成
  - 4.职责链模式：缓存、重试、建立连接等功能存在于拦截器中网络请求相关，主要是网络请求优化。网络请求的时候遇到的问题
  - 5.博客推荐：**[Android数据层架构的实现 上篇](https://www.jianshu.com/p/60e5ebf0096a)**、**[Android数据层架构的实现 下篇](https://www.jianshu.com/p/5def7b42d223)**
- 3.okio
  - 1.简介；
    - 1.sink：自己--》别人
    - 2.source：别人--》自己
    - 3.BufferSink：有缓存区域的sink
    - 4.BufferSource：有缓存区域的source
    - 5.Buffer：实现了3、4的缓存区域，内部有Segment的双向链表，在在转移数据的时候，只需要将指针转移指向就行
  - 2.比java io的好处：
    - 1.减少内存申请和数据拷贝
    - 2.类少，功能齐全，开发效率高
  - 3.内部实现：
    - 1.Buffer的Segment双向链表，减少数据拷贝
    - 2.Segment的内部byte数组的共享，减少数据拷贝
    - 3.SegmentPool的共享和回收Segment
    - 4.sink和source中被实际操作的其实是Buffer，Buffer可以充当sink和source
    - 5.最终okio只是对java io的封装，所有操作都是基于java io 的

> 写在最后:能看到这里的人,我挺佩服你的.这篇文章是我在**头条**面试之前整理的,最后**80%**的题目都命中了,所以祝你好运.

不贩卖焦虑，也不标题党。分享一些这个世界上有意思的事情。题材包括且不限于：科幻、科学、科技、互联网、程序员、计算机编程。下面是我的微信公众号：**世界上有意思的事**，干货多多等你来看。

作者：何时夕
链接：https://www.jianshu.com/p/cf5092fa2694
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# [百度面试题](https://www.jianshu.com/p/f0d2ed1254a9)

电话面试

```
1. 安卓View绘制流程
2. 事件分发机制
3. JAVA基础思想
4. 多线程和安全问题
5. 安卓性能优化和兼容问题
6. 再问一下常规的组件相关问题
```

现场笔试

```kotlin
1  请描述安卓四大组建之间的关系，并说下安卓MVC的设计模式。

2 线程中sleep()和wait()有和却别，各有什么含义                   

3  abstract和interface的区别?

4 array,arrayList, List ,三者有何区别？

5 hashtable和hashmap的区别,并简述Hashmap的实现原理

6 StringBuilder和],String ,subString方法的细微差别

7 请写出四种以上你知道的设计模式，并介绍下实现原理

8 安卓子线程是否能更新UI，如果能请说明具体细节。

9 ANR产生的原因和解决步骤

10 JavaGC机制的原理和内存泄露

11  安卓布局优化方案，          

12  请在100个电话号码找出135的电话号码   注意 不能用正则，（类似怎么最好的遍历LogGat日志）
13  Handler机制，请写出一种更新UI的方法和代码

14  请解释安卓为啥要加签名机制。

15   你觉得安卓开发最关键的技术在哪里？
13  Handler机制，请写出一种更新UI的方法和代码

14  请解释安卓为啥要加签名机制。

15   你觉得安卓开发最关键的技术在哪里？
```

一轮面试：

```undefined
1  ANR 具体产生的类型有哪些，具体说下其产生的最大超时时间。

2  多线程多点下载的过程

3 http协议的理解和用法

4 安卓解决线程并发问题

5 你知道的数据结构有哪些，说下具体实现机制

6 十六进制数据怎么和十进制和二进制之间转换

7 谈下对Java OOP中多态的理解

8  activty和Fragmengt之间怎么通信，Fragmengt和Fragmengt怎么通信

9 怎么让自己的进程不被第三方应用杀掉，系统杀掉之后怎么能启动起来。
10 说下平时开发中比较注意的一些问题，

答 ：可以熟说下svn和git的细节，和代码规范问题，和一些安全信息的问题等

11 自定义view效率高于xml定义吗？说明理由。

13 广播注册一般有几种，各有什么优缺点

14 服务启动一般有几种，服务和activty之间怎么通信，服务和服务之间怎么通信
15 布局优化主要哪些？具体优化？

16 数据库的知识，包括本地数据库优化点。
```

二轮面试

```css
1 安卓事件分发机制，请详细说下整个流程

2 安卓view绘制机制和加载过程，请详细说下整个流程

3 activty的加载过程 请详细介绍下（不是生命周期切记）

4 安卓采用自动垃圾回收机制，请说下安卓内存管理的原理

5  说下安卓虚拟机和java虚拟机的原理和不同点 

6 多线程中的安全队列一般通过什么实现？线程池原理？（java）

7 安卓权限管理，为何在清单中注册权限，安卓APP就可以使用，反之不可以（操作系统）

8  socket短线重连怎么实现，心跳机制又是怎样实现，四次握手步骤有哪些（网络通讯原理）

9 http中TCP和UDP有啥区别，说下HTTP请求的IP报文结构（计算机网络）
10 你知道的安全加密有哪些？   （如果你说了一个加密，面试官就会接着跟进提问，所以之前你必须要会，不会的话背也要背下来）（安全加密）
11  你知道的数据存储结构？堆栈和链表内部机制。（数据结构）

12 说下Linux进程和线程的区别。进程调度优先级，和cpu调度进程关系。（操作系统）

13 请你详细说下你知道的一种设计模式，并解释下java的高内聚和低耦合。

14  spring 的反射和代理，在安卓中应用场景（插件和ROM数据框架）

15 JNI 调用过程中 混淆问题

16 看过安卓源码吗，请说出一个你看过的API或者组建内部原理。

17 android 5.0 6.0 以及7.0预测新特性

18 hybrid混合开发，响应式编程等

17为啥离职呢  对待加班看法

18 你擅长什么，做了那些东西。
```

# [名企面试题](https://www.jianshu.com/p/735be5ece9e8)

## [Android](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/tree/master/android)

##### 基础

- [Android 源码中的设计模式(你需要知道的设计模式全在这里)](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 源码中的设计模式(你需要知道的设计模式全在这里).md)
- [全面了解Activity](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/全面了解Activity.md)
- [Service全面总结](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Service全面总结.md)
- [IntentService使用详解和实例介绍](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/IntentService使用详解和实例介绍.md)
- [Fragment 全解析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Fragment 全解析.md)
- [ContentProvider实例详解](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/ContentProvider实例详解.md)
- [BroadcastReceiver使用总结](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/BroadcastReceiver使用总结.md)
- [Android异步任务机制之AsycTask](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android异步任务机制之AsycTask.md)
- [Android启动过程图解](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android启动过程图解.md)
- [Android 自定义View入门](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 自定义View入门.md)
- [Android 自定义ViewGroup入门实践](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 自定义ViewGroup入门实践.md)
- [Android 缓存机制](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 缓存机制.md)
- [Android 数据存储五种方式使用与总结](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 数据存储五种方式使用与总结.md)
- [Android 异步消息处理机制源码解析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 异步消息处理机制（Handler 、 Looper 、MessageQueue）源码解析.md)
- [Android View事件分发机制源码分析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android View事件分发机制源码分析.md)
- [Android SQLite的使用入门](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android SQLite的使用入门.md)
- [AIDL的使用情况和实例介绍](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/AIDL的使用情况和实例介绍.md)
- [Android 名企面试题及答案整理（一）](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android 名企面试题及答案整理（一）.md)

##### 问题

- [Android5.0、6.0、7.0新特性](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android5.0、6.0、7.0新特性.md)
- [Android中弱引用与软引用的应用场景](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android中弱引用与软引用的应用场景.md)
- [Android长连接，怎么处理心跳机制](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Android长连接，怎么处理心跳机制.md)
- [Asset目录与res目录的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Asset目录与res目录的区别.md)
- [Binder机制原理和底层实现](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Binder机制原理和底层实现.md)
- [Json优劣势](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/Json优劣势.md)
- [ListView优化](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/ListView优化.md)
- [android中图片缓存](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/android中图片缓存.md)
- [两类动画](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/两类动画.md)
- [五大布局易混淆知识](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/五大布局易混淆知识.md)
- [保证service不被杀死](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/保证service不被杀死.md)
- [加速启动activity](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/加速启动activity.md)
- [怎样退出终止App](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/怎样退出终止App.md)
- [activity切换动画](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/android/activity切换动画.md)

##### 外链

- [布局性能优化(include, viewstub, merge)](https://link.jianshu.com?t=http://www.trinea.cn/android/layout-performance/)
- [DOM、SAX、Pull解析XML](https://link.jianshu.com?t=http://www.cnblogs.com/xiaoluo501395377/p/3444744.html)

## [Java](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/tree/master/java)

##### 基础

- [ArrayList、LinkedList、Vector的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] ArrayList、LinkedList、Vector的区别.md)
- [Collection包结构，与Collections的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Collection包结构，与Collections的区别.md)
- [Excption与Error包结构,OOM和SOF](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Excption与Error包结构%2COOM和SOF.md)
- [HashMap和HashTable的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] HashMap和HashTable的区别.md)
- [HashMap源码分析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] HashMap源码分析.md)
- [Hashcode的作用](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Hashcode的作用.md)
- [Map、Set、List、Queue、Stack的特点与用法](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Map、Set、List、Queue、Stack的特点与用法.md)
- [Object有哪些公用方法？](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Object有哪些公用方法？.md)
- [Override和Overload的使用规则和区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Override和Overload的使用规则和区别.md)
- [Switch能否用string做参数？](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] Switch能否用string做参数？.md)
- [ThreadLocal的使用规则和源码分析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] ThreadLocal的使用规则和源码分析.md)
- [ThreadPool用法与示例](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] ThreadPool用法与示例.md)
- [equals与==的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] equals与%3D%3D的区别.md)
- [try catch finally，try里有return，finally还执行么？](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] try catch finally，try里有return，finally还执行么？.md)
- [九种基本数据类型的大小，以及他们的封装类](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 九种基本数据类型的大小，以及他们的封装类.md)
- [从源码分析String、StringBuffer与StringBuilder区别和联系](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 从源码分析String、StringBuffer与StringBuilder区别和联系.md)
- [多线程下生产者消费者问题的五种同步方法实现](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 多线程下生产者消费者问题的五种同步方法实现.md)
- [实现多线程的两种方法](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 实现多线程的两种方法.md)
- [接口（Interface）与 抽象类 （Abstract）使用规则和区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 接口（Interface）与 抽象类 （Abstract）使用规则和区别.md)
- [方法锁、对象锁和类锁的意义和区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 方法锁、对象锁和类锁的意义和区别.md)
- [四种引用，强弱软虚，用到的场景](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 的四种引用，强弱软虚，用到的场景.md)
- [线程同步的方法：sychronized、lock、reentrantLock分析](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 线程同步的方法：sychronized、lock、reentrantLock分析.md)
- [集合框架的层次结构和使用规则梳理](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 集合框架的层次结构和使用规则梳理.md)
- [面向对象的三个特征与含义](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[Java] 面向对象的三个特征与含义.md)
- [static的作用和意义](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[java] static的作用和意义.md)
- [多态实现的JVM调用过程](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/[java] 多态实现的JVM调用过程.md)
- [wait()和sleep()的区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/wait()和sleep()的区别.md)
- [git命令使用](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/git命令使用.md)
- [Java与C++对比](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/java/Java与C%2B%2B对比.md)

##### 外链

- [java反射](https://link.jianshu.com?t=http://762626559-qq-com.iteye.com/blog/395402)
- [java回调](https://link.jianshu.com?t=http://blog.csdn.net/xiaanming/article/details/8703708/)
- [Java泛型](https://link.jianshu.com?t=http://www.weixueyuan.net/view/6321.html)
- [java 新特性](https://link.jianshu.com?t=http://www.cnblogs.com/langtianya/p/3757993.html)
- [Java IO与NIO](https://link.jianshu.com?t=http://www.iteye.com/topic/834447)
- [foreach与正常for循环效率对比](https://link.jianshu.com?t=http://www.xuebuyuan.com/780786.html)

## [数据结构](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/tree/master/data structure)

##### 基础

- [九大基础排序总结与对比(排序算法一网打尽)](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 九大基础排序总结与对比.md)
- [AVL树和AVL旋转、哈夫曼树和哈夫曼编码](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] AVL树和AVL旋转、哈夫曼树和哈夫曼编码.md)
- [B(B-)树、B+树、B树](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] B(B-)树、B%2B树、B树.md)
- [Hash表、Hash函数及冲突解决](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] Hash表、Hash函数及冲突解决.md)
- [KMP的一个简单解释](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] KMP的一个简单解释.md)
- [二分查找与变种二分查找](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 二分查找与变种二分查找.md)
- [二叉树前中后、层次遍历算法](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 二叉树前中后、层次遍历算法.md)
- [图的BFS、DFS、prim、Dijkstra算法](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 图的BFS、DFS、prim、Dijkstra算法.md)
- [字符串操作](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 字符串操作.md)
- [数组与链表的优缺点和区别](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 数组与链表的优缺点和区别.md)
- [红黑树](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 红黑树.md)
- [队列和栈](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/data structure/[数据结构] 队列和栈.md)

##### 外链

- [海量数据处理 ](https://link.jianshu.com?t=http://blog.csdn.net/zyq522376829/article/details/47686867)

## [算法](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/tree/master/algorithm)

##### 基础

- [二叉搜索树与双向链表](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/二叉搜索树与双向链表.md)
- [二叉树中 和为某值 的所有路径](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/二叉树中 和为某值 的所有路径.md)
- [二叉树的镜像](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/二叉树的镜像.md)
- [二维数组中的查找](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/二维数组中的查找.md)
- [二进制中1的个数](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/二进制中1的个数.md)
- [从上往下打印二叉树](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/从上往下打印二叉树.md)
- [从尾到头打印链表](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/从尾到头打印链表.md)
- [判断二叉搜索树的后序遍历序列](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/判断二叉搜索树的后序遍历序列.md)
- [判断栈的弹出序列](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/判断栈的弹出序列.md)
- [判断树B是不是树A的子结构](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/判断树B是不是树A的子结构.md)
- [包含min函数的栈](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/包含min函数的栈.md)
- [反转链表](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/反转链表.md)
- [变态跳台阶](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/变态跳台阶.md)
- [合并两个排序链表](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/合并两个排序链表.md)
- [复杂链表的复制](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/复杂链表的复制.md)
- [字符串中空格替换](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/字符串中空格替换.md)
- [字符串的顺序全排列](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/字符串的顺序全排列.md)
- [数组中出现次数超过一半的数字](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/数组中出现次数超过一半的数字.md)
- [斐波那契数列](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/斐波那契数列.md)
- [旋转数组的最小数字](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/旋转数组的最小数字.md)
- [浮点数的整数次方](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/浮点数的整数次方.md)
- [用两个栈实现队列](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/用两个栈实现队列.md)
- [矩形覆盖](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/矩形覆盖.md)
- [调整数组顺序使奇数位于偶数前面](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/调整数组顺序使奇数位于偶数前面.md)
- [跳台阶](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/跳台阶.md)
- [重建二叉树](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/重建二叉树.md)
- [链表中倒数第k个结点](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/链表中倒数第k个结点.md)
- [顺时针打印矩阵](https://link.jianshu.com?t=https://github.com/helen-x/AndroidInterview/blob/master/algorithm/swordForOffer/顺时针打印矩阵.md)

##### 外链

- [分治算法](https://link.jianshu.com?t=http://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741370.html)
- [动态规划](https://link.jianshu.com?t=http://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741374.html)
- [贪心算法](https://link.jianshu.com?t=http://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741375.html)
- [分支限界法](https://link.jianshu.com?t=http://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741378.html)

作者：菜刀文
链接：https://www.jianshu.com/p/735be5ece9e8
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# 面试和必备的技能

这里只简单列举一些东西，可能不是特别全，但是却特别适用，也不一定按照下面的流程，有可能是穿插的，也有可能都有，根据公司的规模以及面试官的心情而定（哈哈哈 ，你们就自求多福吧）。建议大家还是要将下面的东西全部掌握，没事写写代码，练练手，在项目中能用到的地方一定要用，有可能会遇到很多坑，一定要自己想办法填坑，之后回忆起这段经历，肯定可以敢理直气壮的跟别人讨论。如果你说的头头是道，那么对方会先输一层，然后在心里对你佩服。

1. 一般情况下第一轮都是基础面试，需要扎实的基础
   
   - 最常用的Android 基础知识
   - Java 基础知识
   - 了解一些 常用东西的原理，例如：handler， thread 等
   - 项目中的技术点

2. 第二轮的时候需要了解更深层次的东西
   
   - Android 事件分发机制原理
   - Android 绘图机制原理
   - WindowManager 的相关知识
   - 进程间传输方式
   - Java 内存管理机制
   - 一些常用的 list,map 原理，以及子类之间的差别

3. 能进入第三轮基本没什么问题，但是要注意以下问题
   
   - 该轮一般是 老大或者部门负责人，问的问题一般都看 深度与广度
   
   - 当问及薪水的时候，要说一个合适的，小公司随意，大公司一定要慎重，当心里没底的时候，可以告诉对方，让对方给一个合理的薪资。一般都是在原工资基础之上增长，听猎头说一般涨幅都在15%-30%，超 NB 的可以要30%及以上，如果感觉自己还不错的，挺厉害的，建议最高20%，一般人就定在15% 左右最靠谱。公司内部一般有一套机制，根据公司情况而定。
   
   - 我们的面试原则就是拿到合理薪资，得到 offer
   
   - 个人发展情况，这个问题很难回答，如果和公司方向不符合，极有可能和公司无缘。建议多试探性的问问公司缺少什么，你能否给予公司对应的东西。当然对于有自我追求的人，那可以放心大胆的提。我的方向就是架构师，哈哈哈，挺极端的，别学我哦。我感觉选择都是双向的，因此我知道自己需要的是什么。
   
   - 你最擅长什么UI 还是其他什么？这个问题更不好回答。你要说你擅长 UI，是不是意味着你其他能力就不行？虽然我不知道面试官的用意，但是我能感觉到，这个问题不是那么好回答，我会回答说自己都行，来什么业务接什么需求。可能回答不太好，总之和公司的职位吻合就行，这样总不至于出错吧。

## [Android 高级面试题及答案](https://www.cnblogs.com/deman/p/5860976.html)

- [1.如何对 Android 应用进行性能分析](https://www.cnblogs.com/deman/p/5860976.html#_label0)
- [2.什么情况下会导致内存泄露](https://www.cnblogs.com/deman/p/5860976.html#_label1)
- [3.如何避免 OOM 异常](https://www.cnblogs.com/deman/p/5860976.html#_label2)
- [4.Android 中如何捕获未捕获的异常](https://www.cnblogs.com/deman/p/5860976.html#_label3)
- [5.ANR 是什么？怎样避免和解决 ANR（重要）](https://www.cnblogs.com/deman/p/5860976.html#_label4)
- [6.Android 线程间通信有哪几种方式](https://www.cnblogs.com/deman/p/5860976.html#_label5)
- [7.Devik 进程，linux 进程，线程的区别](https://www.cnblogs.com/deman/p/5860976.html#_label6)
- [8.描述一下 android 的系统架构](https://www.cnblogs.com/deman/p/5860976.html#_label7)
- [9.android 应用对内存是如何限制的?我们应该如何合理使用内存？](https://www.cnblogs.com/deman/p/5860976.html#_label8)
- [10. 简述 android 应用程序结构是哪些](https://www.cnblogs.com/deman/p/5860976.html#_label9)
- [11.请解释下 Android 程序运行时权限与文件系统权限的区别](https://www.cnblogs.com/deman/p/5860976.html#_label10)
- [12.Framework 工作方式及原理，Activity 是如何生成一个 view 的，机制是什么](https://www.cnblogs.com/deman/p/5860976.html#_label11)
- [13.多线程间通信和多进程之间通信有什么不同，分别怎么实现](https://www.cnblogs.com/deman/p/5860976.html#_label12)
- [14.Android 屏幕适配](https://www.cnblogs.com/deman/p/5860976.html#_label13)
- [15.什么是 AIDL 以及如何使用](https://www.cnblogs.com/deman/p/5860976.html#_label14)
- [16.Handler 机制](https://www.cnblogs.com/deman/p/5860976.html#_label15)
- [17.事件分发机制](https://www.cnblogs.com/deman/p/5860976.html#_label16)
- [18.子线程发消息到主线程进行更新 UI，除了 handler 和 AsyncTask，还有什么](https://www.cnblogs.com/deman/p/5860976.html#_label17)
- [19.子线程中能不能 new handler？为什么](https://www.cnblogs.com/deman/p/5860976.html#_label18)
- [20.Android 中的动画有哪几类，它们的特点和区别是什么](https://www.cnblogs.com/deman/p/5860976.html#_label19)
- [21.如何修改 Activity 进入和退出动画](https://www.cnblogs.com/deman/p/5860976.html#_label20)
- [22.SurfaceView & View 的区别](https://www.cnblogs.com/deman/p/5860976.html#_label21)
- [23.开发中都使用过哪些框架、平台](https://www.cnblogs.com/deman/p/5860976.html#_label22)
- [24.使用过那些自定义View](https://www.cnblogs.com/deman/p/5860976.html#_label23)
- [25.自定义控件：绘制圆环的实现过程](https://www.cnblogs.com/deman/p/5860976.html#_label24)
- [26.自定义控件：摩天轮的实现过程](https://www.cnblogs.com/deman/p/5860976.html#_label25)
- [28.流式布局的实现过程](https://www.cnblogs.com/deman/p/5860976.html#_label27)
- [29.第三方登陆](https://www.cnblogs.com/deman/p/5860976.html#_label28)
- [30.第三方支付](