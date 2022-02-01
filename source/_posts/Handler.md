---
title: Handler
comments: true
date: 2017-08-21 17:06:35
tags:
categories: ANDROID

---



#### Handler作用

既然process内的thread share their memories. why we need handler.我们直接可以 subThread get data,赋值给Ui thread的变量呀。



正常来说是没问题的。但是像下面这种情况。

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "123";
    public static int anInt = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 子
        new Thread(new Runnable() {
            @Override
            public void run() {
                anInt = 2;
                Log.i(TAG, "子线程: " + anInt);
            }
        }).start();
        // 主
            Log.i(TAG, "主线程: " + anInt);
      
    }
}
————————————————
版权声明：本文为CSDN博主「dev晴天」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_38350635/article/details/90440833
```

如上：主线程定义了anInt，子线程对其修改。我们再在主线程打印，能得到修改后的anInt = 2 吗？
 答案：不确定
 原因：线程争夺cup使用权顺序不确定，可能,子线程先获得使用权，则两个Log都打印2。当然主线程先获得使用权时主线程的log打印数值为1。



解决方案
1、让子线程先执行。

    比如主线程设置低优先级、主线程休息等等（不靠谱，提供一种想法吧）

2、安卓中的Handler机制

    子线程发送消息，主线程接受消息，只有主线程收到消息再打印处理数值。
    handler使用略。。。。。。

3、接口回调机制





##### 原理图



<img src="Handler/2021-07-07_15-49-54_hanlder.png" />



 ![](https://images.xiaozhuanlan.com/photo/2019/5dd99feb4297e4c3a5ad55dba0812a1a.png)





##### 各组件作用

- Handler：事件的发送及处理者，在构造方法中可以设置其 async，默认为 false。若 async 为 true 则该 Handler 发送的 Message 均为异步消息，有同步屏障的情况下会被优先处理。
- Looper：一个用于遍历 MessageQueue 的类，每个线程有一个独有的 Looper，它会在所处的线程开启一个死循环，不断从 MessageQueue 中拿出消息，并将其发送给 target 进行处理
- MessageQueue：在主线程,用于存储 Message，内部维护了 Message 的链表，每次拿取 Message 时，若该  Message 离真正执行还需要一段时间，会通过 nativePollOnce  进入阻塞状态，避免资源的浪费。若存在消息屏障，则会忽略同步消息优先拿取异步消息，从而实现异步消息的优先消费。





#####  消息入队算法

https://www.jianshu.com/p/9efe3b48b730

https://juejin.cn/post/6844904113470177293

https://www.bilibili.com/video/BV1VE411Z7Ay?p=4





##### ThreadLocal存取算法
　ThreadLocal是一个线程内部的数据存储类，它可以指定线程的存储数据，数据存储后只有在指定的线程种裁可以获取到存储的数据, 其他线程无法获取。

 验证:


      private ThreadLocal<String> mStringThreadLocal = new ThreadLocal<>();
      mStringThreadLocal.set("我是主线程");
        Log.d(TAG, "mStringThreadLocal 主线程 :　" + mStringThreadLocal.get());
        exec.submit(new Runnable() {
            @Override
            public void run() {
                mStringThreadLocal.set("我是线程１");
                Log.d(TAG, "mStringThreadLocal 线程１:　" + mStringThreadLocal.get());
            }
        });
    
        exec.submit(new Runnable() {
            @Override
            public void run() {
                Log.d(TAG, "mStringThreadLocal 线程2: " + mStringThreadLocal.get());
            }
        });

运行结果:
>       D/MainActivity: mStringThreadLocal 主线程 :　我是主线程
     D/MainActivity: mStringThreadLocal 线程１:　我是线程１
     D/MainActivity: mStringThreadLocal 线程2: null

从日志可以看到，虽然在不同线程访问的同一个ThreadLocal对象，但是他们通过ThreadLocal获取到的值确不一样
　结论：　对ThreadLocal所做的　读写操作仅限于各自线程的内部，ThreadLocal可以在多个线程互不干扰的存储和修改数据。
  !!!任务： 分析ThreadLocal存取算法

[^参考：Android开发艺术探索　p377]

#####  消息

###### 同步消息

 来个简单的例子  
 ```
    new Thread(new Runnable() {
            @Override
            public void run() {
              Looper.prepare();
              handler = new Handler();
              Looper.loop();
            }
        }).start();
 ```
把　Looper.prepare();注释，报错日志 : `java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepare()`

从报错信息入手看原因，初始化Handler (API = 25)

```
 public Handler(Callback callback, boolean async) {
        if (FIND_POTENTIAL_LEAKS) {
            final Class<? extends Handler> klass = getClass();
            if ((klass.isAnonymousClass() || klass.isMemberClass() || klass.isLocalClass()) &&
                    (klass.getModifiers() & Modifier.STATIC) == 0) {
                Log.w(TAG, "The following Handler class should be static or leaks might occur: " +
                    klass.getCanonicalName());
            }
        }

        mLooper = Looper.myLooper();
        if (mLooper == null) {
            throw new RuntimeException(
                "Can't create handler inside thread that has not called Looper.prepare()");  //报错信息
        }
        mQueue = mLooper.mQueue;
        mCallback = callback;
        mAsynchronous = async;
    }
```
　mLooper为空,所以报错，看下`Looper.myLooper();`

```
 /**
     * Return the Looper object associated with the current thread.  Returns
     * null if the calling thread is not associated with a Looper.
     */
    public static @Nullable Looper myLooper() {
        return sThreadLocal.get();
    }
```
  注释可以看到，当前线程没有Looper对象, 那么Looper对象是怎么创建的呢，很显然就是上面例子里的` Looper.prepare();`

```
 /** Initialize the current thread as a looper.
      * This gives you a chance to create handlers that then reference
      * this looper, before actually starting the loop. Be sure to call
      * {@link #loop()} after calling this method, and end it by calling
      * {@link #quit()}.
      */
    public static void prepare() {
        prepare(true);
    }

    private static void prepare(boolean quitAllowed) {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        sThreadLocal.set(new Looper(quitAllowed));
    }
```
如果没有Looper　则new 一个，由此也看到每个线程最多一个Looper对象,接着看初始化的Looper
```
 private Looper(boolean quitAllowed) {
        mQueue = new MessageQueue(quitAllowed);
        mThread = Thread.currentThread();
    }
```
MesageQueue也是通过Looper创建的，看到private 构造方法,一个Looper管理一个　ＭesageQueue,
但是我们通常在UI线程初始化Handler不需要调用 `Looper.prepare();`,这是因为应用启动后,主线程
ActivityThread main方法为我们做了工作。

```
 public static void main(String[] args) {
        Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");
        SamplingProfilerIntegration.start();

        // CloseGuard defaults to true and can be quite spammy.  We
        // disable it here, but selectively enable it later (via
        // StrictMode) on debug builds, but using DropBox, not logs.
        CloseGuard.setEnabled(false);

        Environment.initForCurrentUser();

        // Set the reporter for event logging in libcore
        EventLogger.setReporter(new EventLoggingReporter());

        // Make sure TrustedCertificateStore looks in the right place for CA certificates
        final File configDir = Environment.getUserConfigDirectory(UserHandle.myUserId());
        TrustedCertificateStore.setDefaultUserDirectory(configDir);

        Process.setArgV0("<pre-initialized>");

        Looper.prepareMainLooper();

        ActivityThread thread = new ActivityThread();
        thread.attach(false);

        if (sMainThreadHandler == null) {
            sMainThreadHandler = thread.getHandler();
        }

        if (false) {
            Looper.myLooper().setMessageLogging(new
                    LogPrinter(Log.DEBUG, "ActivityThread"));
        }

        // End of event ActivityThreadMain.
        Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
        Looper.loop();

        throw new RuntimeException("Main thread loop unexpectedly exited");
    }
```
这样初始化Handler操作分析好了，接着就开始发消息了 `handler.sendMessage(message);`,经过层层调用来到这个方法

```
　 public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
        MessageQueue queue = mQueue;
        if (queue == null) {
            RuntimeException e = new RuntimeException(
                    this + " sendMessageAtTime() called with no mQueue");
            Log.w("Looper", e.getMessage(), e);
            return false;
        }
        return enqueueMessage(queue, msg, uptimeMillis);
    }
```
第二个参数时间是可以添加延时的参数
```
 boolean enqueueMessage(Message msg, long when) {
        if (msg.target == null) {
            throw new IllegalArgumentException("Message must have a target.");
        }
        if (msg.isInUse()) {
            throw new IllegalStateException(msg + " This message is already in use.");
        }

        synchronized (this) {
            if (mQuitting) {
                IllegalStateException e = new IllegalStateException(
                        msg.target + " sending message to a Handler on a dead thread");
                Log.w(TAG, e.getMessage(), e);
                msg.recycle();
                return false;
            }

            msg.markInUse();
            msg.when = when;
            Message p = mMessages;
            boolean needWake;
            if (p == null || when == 0 || when < p.when) {
                // New head, wake up the event queue if blocked.
                msg.next = p;
                mMessages = msg;
                needWake = mBlocked;
            } else {
                // Inserted within the middle of the queue.  Usually we don't have to wake
                // up the event queue unless there is a barrier at the head of the queue
                // and the message is the earliest asynchronous message in the queue.
                needWake = mBlocked && p.target == null && msg.isAsynchronous();
                Message prev;
                for (;;) {
                    prev = p;
                    p = p.next;
                    if (p == null || when < p.when) {
                        break;
                    }
                    if (needWake && p.isAsynchronous()) {
                        needWake = false;
                    }
                }
                msg.next = p; // invariant: p == prev.next
                prev.next = msg;
            }

            // We can assume mPtr != 0 because mQuitting is false.
            if (needWake) {
                nativeWake(mPtr);
            }
        }
        return true;
    }
```
这要Mesage消息就添加到 MesageQueue中了，按事件顺序添加，然后就是取消息了，通过调用`Looper.loop();`
```
 /**
     * Run the message queue in this thread. Be sure to call
     * {@link #quit()} to end the loop.
     */
    public static void loop() {
        final Looper me = myLooper();
        if (me == null) {
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
        }
        final MessageQueue queue = me.mQueue;

        // Make sure the identity of this thread is that of the local process,
        // and keep track of what that identity token actually is.
        Binder.clearCallingIdentity();
        final long ident = Binder.clearCallingIdentity();

        for (;;) {
            Message msg = queue.next(); // might block
            if (msg == null) {
                // No message indicates that the message queue is quitting.
                return;
            }

            // This must be in a local variable, in case a UI event sets the logger
            final Printer logging = me.mLogging;
            if (logging != null) {
                logging.println(">>>>> Dispatching to " + msg.target + " " +
                        msg.callback + ": " + msg.what);
            }

            final long traceTag = me.mTraceTag;
            if (traceTag != 0 && Trace.isTagEnabled(traceTag)) {
                Trace.traceBegin(traceTag, msg.target.getTraceName(msg));
            }
            try {
                msg.target.dispatchMessage(msg);
            } finally {
                if (traceTag != 0) {
                    Trace.traceEnd(traceTag);
                }
            }

            if (logging != null) {
                logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
            }

            // Make sure that during the course of dispatching the
            // identity of the thread wasn't corrupted.
            final long newIdent = Binder.clearCallingIdentity();
            if (ident != newIdent) {
                Log.wtf(TAG, "Thread identity changed from 0x"
                        + Long.toHexString(ident) + " to 0x"
                        + Long.toHexString(newIdent) + " while dispatching to "
                        + msg.target.getClass().getName() + " "
                        + msg.callback + " what=" + msg.what);
            }

            msg.recycleUnchecked();
        }
    }
```
通过 Message msg = queue.next();　获取消息，然后用`  msg.target.dispatchMessage(msg);`把消息往哪发呢 ?
```
/**
     * Handle system messages here.
     */
    public void dispatchMessage(Message msg) {
        if (msg.callback != null) {
            handleCallback(msg);
        } else {
            if (mCallback != null) {
                if (mCallback.handleMessage(msg)) {
                    return;
                }
            }
            handleMessage(msg);
        }
    }
```
点开　handleMesage(msg),msg.target 就是Handler

```
 /**
     * Subclasses must implement this to receive messages.
     */
    public void handleMessage(Message msg) {
    }
```
这里就清楚了，消息就传到了　Hanlder里面handleMessage的实现方法
从上面分析handler消息传递基本了解了，借张图看下整片森林



###### 异步消息

Android系统16ms会刷新一次屏幕，如果主线程的消息过多，在16ms之内没有执行完，必然会造成卡顿或者掉帧。那怎么才能不排队，没有延时的处理呢？这个时候就需要异步消息，在处理异步消息的时候，我们就需要同步屏障，让异步消息不用排队等候处理。可以理解为同步屏障是一堵墙，把同步消息队列拦住，先处理异步消息，等异步消息处理完了，这堵墙就会取消，然后继续处理同步消息。

https://blog.csdn.net/ly502541243/article/details/109091386

查找异步消息方法

MessageQueue.java 

```java
 Message next() {
        for (;;) {
            if (nextPollTimeoutMillis != 0) {
                Binder.flushPendingCommands();
            }

            nativePollOnce(ptr, nextPollTimeoutMillis);

            synchronized (this) {
                // Try to retrieve the next message.  Return if found.
                final long now = SystemClock.uptimeMillis();
                Message prevMsg = null;
                Message msg = mMessages;
                if (msg != null && msg.target == null) {
                    // Stalled by a barrier.  Find the next asynchronous message in the queue.
                    do {
                        prevMsg = msg;
                        msg = msg.next;
                    } while (msg != null && !msg.isAsynchronous()); //循环查找 直到找到异步消息为止.
                }
            }
        }
```



https://blog.csdn.net/ly502541243/article/details/109091386





#####  发送延时消息

看下面问题:handler如何实现延时发消息postdelay()

https://zhuanlan.zhihu.com/p/260661053

https://blog.csdn.net/thh159/article/details/103644489





##### IdleHandler使用

当消息队列空闲时会执行IdleHandler的queueIdle()方法，该方法返回一个boolean值，如果为false则执行完毕之后移除这条消息，如果为true则保留，等到下次空闲时会再次执行，下面看下MessageQueue的next()方法可以发现确实是这样

```java
Message next() {
    // Run the idle handlers.
            // We only ever reach this code block during the first iteration.
 	keep = idler.queueIdle();
}
```

1. Activity启动优化：onCreate，onStart，onResume中耗时较短但非必要的代码可以放到IdleHandler中执行，减少启动时间

2. 想要在一个View绘制完成之后添加其他依赖于这个View的View，当然这个用View#post()也能实现，区别就是前者会在消息队列空闲时执行
3. 一些第三方库中有使用，比如LeakCanary，Glide中有使用到，具体可以自行去查看
4. 以前我们在Activity中获取某个控件的宽高的时候总是得到的是0，那是因为view的测量还未完成。通常的做法是监听ViewTreeObserver，它是在ViewRootImpl测量完成之后调用ViewTreeObserver.dispatchOnGlobalLayout()方法，这时候在onGlobalLayout回调中获取的控件宽高都是正确的数据。
   

```kotlin
Looper.myQueue().addIdleHandler {
    Log.i("HandlerActivity", "width ${btFive.width}  height ${btFive.height}")
    false
}
```

https://blog.csdn.net/ZYJWR/article/details/103086664

https://www.wanandroid.com/wenda/show/8723



##### 问题

###### handler的是怎样实现的？

新线程中执行了一个耗时操作，然后把该结果塞给message，handler将发送这个messageQueue，Loop不停的从消息队列中取出消息。Handler 分发给Ui.



###### Handler是怎么切换线程的

我们在不同的线程发送消息，线程之间的资源是共享的



###### Handler机制了解吗？一个线程有几个Looper？为什么？

只能有一个，不然调用Looper.prepare()会抛出运行时异常，提示“Only one Looper may be created per thread”



###### handler如何实现延时发消息postdelay(),

可以看到这里也是一个for循环遍历队列，核心变量就是nextPollTimeoutMillis。可以看到，计算出nextPollTimeoutMillis后就调用nativiePollOnce这个native方法。这里的话大概可以猜到他的运行机制，因为他是根据执行时间进行排序的，那传入的这个nextPollTimeoutMillis应该就是休眠时间，类似于java的sleep(time)。休眠到下一次message的时候就执行。那如果我在这段时间又插入了一个新的message怎么办，所以handler每次插入message都会唤醒线程，重新计算插入后，再走一次这个休眠流程。



值得注意的是这个方法没有开启子线程，只是调用了run(), 在` msg.target.dispatchMessage(msg)`可以看到，接着调用了handleCallback().

https://www.jianshu.com/p/68083d432b3f



###### 如果移除一个延时消息会解除休眠吗





###### 主线程死循环不会卡死吗

从前面的主线程、子线程的分析可以看出，Looper会在线程中不断的检索消息，如果是子线程的Looper死循环，一旦任务完成，用户应该手动退出，而不是让其一直休眠等待。（引用自Gityuan）线程其实就是一段可执行的代码，当可执行的代码执行完成后，线程的生命周期便该终止了，线程退出。而对于主线程，我们是绝不希望会被运行一段时间，自己就退出，那么如何保证能一直存活呢？简单做法就是可执行代码是能一直执行下去的，死循环便能保证不会被退出，例如，binder 线程也是采用死循环的方法，通过循环方式不同与 Binder 驱动进行读写操作，当然并非简单地死循环，无消息时会休眠。**Android是基于消息处理机制的，用户的行为都在这个Looper循环中，我们在休眠时点击屏幕，便唤醒主线程继续进行工作**。

主线程的死循环一直运行是不是特别消耗 CPU 资源呢？ 其实不然，这里就涉及到 Linux pipe/epoll机制，简单说就是在主线程的 MessageQueue 没有消息时，便阻塞在 loop 的 queue.next() 中的 nativePollOnce() 方法里，此时主线程会释放 CPU 资源进入休眠状态，直到下个消息到达或者有事务发生，通过往 pipe 管道写端写入数据来唤醒主线程工作。这里采用的 epoll 机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。

https://www.jianshu.com/p/a7969d73c120





###### handler内存泄露问题



###### 说说你对Handler机制的了解，同步消息，异步消息等



###### IdleHandler用过吗,IdleHandler应用场景？

 见上面IdleHandler使用

handler如何实现延时发消息postdelay()。callback，runnable，msg的执行优先级。阻塞是怎么实现的？为什么不会阻塞主线程？

###### 

###### Handler休眠是怎样的？epoll的原理是什么？

###### 如何实现延时消息，如果移除一个延时消息会解除休眠吗？



Handler内存泄漏的GCRoot是什么？

epoll的时候算是卡顿吗

怎么样算是卡顿了

怎么利用消息机制检测卡顿

除了这种方式还有别的监测卡顿的方式吗



###### Android中为什么主线程不会因为Looper.loop()里的死循环卡死？

对于线程即是一段可执行的代码，当可执行代码执行完成后，线程生命周期便该终止了，线程退出。而对于主线程，我们是绝不希望会被运行一段时间，自己就退出，那么如何保证能一直存活呢？简单做法就是可执行代码是能一直执行下去的，死循环便能保证不会被退出，例如，binder线程也是采用死循环的方法，通过循环方式不同与Binder驱动进行读写操作，当然并非简单地死循环，无消息时会休眠。但这里可能又引发了另一个问题，既然是死循环又如何去处理其他事务呢？通过创建新线程的方式。真正会卡死主线程的操作是在回调方法`onCreate/onStart/onResume`等操作时间过长，会导致掉帧，甚至发生ANR，looper.loop本身不会导致应用卡死。

主线程的死循环一直运行是不是特别消耗CPU资源呢？ 其实不然，这里就涉及到`Linux pipe/epoll`机制，简单说就是在主线程的`MessageQueue`没有消息时，便阻塞在loop的`queue.next()`中的`nativePollOnce()`方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。 Gityuan–Handler(Native层)



Handler通信，Binder通信



###### Handler同步屏障

Handler发送的消息分为普通消息、屏障消息、异步消息，一旦Looper在处理消息时遇到屏障消息，那么就不再处理普通的消息，而仅仅处理异步的消息。不再使用屏障后，需要撤销屏障，不然就再也执行不到普通消息了。

为什么需要这样？它是设计来为了让某些特殊的消息得以更快被执行的机制。比如绘制界面，这种消息可能会明显的被用户感知到，稍有不慎就会引起卡顿、掉帧之类的，所以需要及时处理（可能消息队列中有大量的消息，如果像平时一样挨个进行处理，那绘制界面这个消息就得等很久，这是不想看到的）。

屏障消息仅仅是起一个屏障的作用，本身一般不附带其他东西，它需要配合其他Handler组件才能发挥作用。  



1、Handler问题三连：是什么？有什么用？为什么要用，不用行不行？

2、Android UI更新机制(GUI) 为何设计成了单线程的？

3、真的只能在主(UI)线程中更新UI吗？

4、真的不能在主(UI)线程中执行网络操作吗？

6、为什么建议使用Message.obtain()来创建Message实例？

7、为什么子线程中不可以直接new Handler()而主线程中可以？

8、主线程给子线程的Handler发送消息怎么写？

9、HandlerThread实现的核心原理？

10、当你用Handler发送一个Message，发生了什么？

11、Looper是怎么拣队列里的消息的？

12、分发给Handler的消息是怎么处理的？

14、Looper在主线程中死循环，为啥不会ANR？

15、Handler泄露的原因及正确写法

16、Handler中的同步屏障机制

17、Android 11 Handler相关变更

https://juejin.cn/post/6844904150140977165



handler post和handleMesage区别

​	**post的runnable会直接在callback中调用run方法执行，而sendMessage方法要用户主动重写mCallback或者handleMessage方法来处理**

---



![](handler/2021-07-31_4.42_hanlder_qustion.png)



https://www.bilibili.com/video/BV1A7411M7zQ?from=search&seid=14693797999095810218

https://juejin.im/post/6844904150140977165



http://blog.csdn.net/guolin_blog/article/details/9991569

https://xiaozhuanlan.com/topic/0843791256





glide handler 

https://www.bilibili.com/video/BV1FU4y1V7HE



https://segmentfault.com/a/1190000039809784