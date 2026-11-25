---
title: Handler
comments: true
date: 2017-08-21 17:06:35
tags:
categories: ANDROID

---

## 复习问题

### 线程、队列和分发

- Handler 对应哪个 Looper？任务由哪条线程执行？
- 同一进程可以共享变量，为什么还要用 Handler？UI 更新为什么由主线程处理？网络操作为什么通常放到后台？
- 一个线程能有几个 Handler、几个 Looper？普通子线程能否创建 Handler？主线程怎样给子线程的 Handler 发消息？
- 多个 Handler 往同一个队列添加消息时，队列怎样保证线程安全？
- `Message` 里面有哪些字段？为什么用 `Message.obtain()`？
- 调用 `sendMessage()` 或 `post()` 后，消息怎样入队、取出和分发？三个回调分支的顺序是什么？
- 同步屏障怎样让异步消息通过？Android 11 的 Handler 有哪些变化？

### 等待、卡顿和泄漏

- Looper 阻塞等消息时，CPU 还能执行其他任务吗？
- `Looper.loop()` 一直循环，为什么通常不会触发 ANR？
- `postDelayed()` 怎样实现延时？移除延时消息时会唤醒 Looper 吗？
- `epoll` 如何让主线程等待和唤醒？正常等待和 UI 卡顿怎样区分？
- 怎样利用消息分发检测卡顿？还可以通过什么方式验证卡顿？
- Handler 相关内存泄漏有哪些场景？引用链从哪个 GC Root 出发？如何清理页面退出后仍待执行的任务？
- IdleHandler 适合做什么？`HandlerThread` 和 `IntentService` 的核心实现是什么？

## 为什么需要 Handler

同一进程里的线程可以共享变量。那为什么还要用 Handler 把结果交回主线程？下面的例子里，子线程把 `anInt` 改成 2，主线程启动子线程后马上读取它。

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "123";
    public static int anInt = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        new Thread(new Runnable() {
            @Override
            public void run() {
                anInt = 2;
                Log.i(TAG, "子线程: " + anInt);
            }
        }).start();

        Log.i(TAG, "主线程: " + anInt);
    }
}
```

版权声明：本文为 CSDN 博主「dev晴天」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。 [原文链接](https://blog.csdn.net/qq_38350635/article/details/90440833)

主线程可能读到 1，也可能读到 2。启动子线程后，两条线程各自继续执行，代码里没有建立从子线程写入到主线程读取的顺序。可以在子线程拿到结果之后，通过绑定到主 Looper 的 Handler 投递更新任务。主线程取到这项任务时，再用结果更新界面。

下面的源码说明以 Android 6.0.1 为例。

## Handler、Looper 和 MessageQueue

| 组件 | 作用 |
| --- | --- |
| `Handler` | 把消息送进它所绑定的 Looper 的队列，并在分发时处理发给自己的消息。 |
| `Looper` | 在所属线程上运行消息循环，从队列取出消息，交给消息指定的 Handler。 |
| `MessageQueue` | 保存待执行的消息，内部用 `Message.next` 把消息连成链表。取消息时也负责等待。 |
| `Message` | 保存消息类型 `what`、数据 `obj`、执行时间 `when`、目标 `target` 等信息。通过 `post()` 发送的 `Runnable` 保存在 `callback` 字段。 |

### 主线程的 Looper 从哪里来

应用启动时，主线程在 `ActivityThread.main()` 中调用 `Looper.prepareMainLooper()`。这一步创建 Looper 和它的消息队列，再把这个 Looper 登记为主 Looper。稍后，同一条主线程调用 `Looper.loop()`，开始取消息并分发。

```java
Handler mainHandler = new Handler(Looper.getMainLooper());
```

构造 Handler 时传入主 Looper，Handler 会保存它和它的 `MessageQueue`。以后，无论在哪条线程调用 `mainHandler.post()`，任务都会进入主线程的队列。主线程在自己的循环里取出任务，再调用它的 `run()`。所以判断任务在哪条线程执行，要看创建 Handler 时选的是哪个 Looper。

### 子线程的 Looper 和 ThreadLocal

Android 用 `ThreadLocal` 保存每条线程自己的 Looper。`Looper.myLooper()` 返回当前线程的值。`Looper.prepare()` 先检查这个值，已经有 Looper 就抛出异常，所以一条线程最多创建一个 Looper。多个 Handler 可以共享这个 Looper 和它的队列。

普通子线程可以直接使用已有的主线程 Handler 来发送消息。要让子线程自己通过 Handler 接收任务，可以在子线程里先调用 `Looper.prepare()`，创建 Handler，再调用 `Looper.loop()`。需要持续处理任务时，也可以使用 `HandlerThread`，它会在自己的线程里完成准备和循环。

```java
new Thread(() -> {
    Looper.prepare();
    Handler handler = new Handler(Looper.myLooper());
    // 在进入循环前，把 handler 安全地交给需要投递任务的一方。
    Looper.loop();
}).start();
```

旧代码常用无参数的 `new Handler()`。它会用 `Looper.myLooper()` 查找当前线程的 Looper。普通子线程如果没有调用过 `Looper.prepare()`，这种写法会抛出异常。子线程的消息循环结束使用后，可以通过 `quit()` 或 `quitSafely()` 退出。

**用日志验证 ThreadLocal**

```java
ThreadLocal<String> local = new ThreadLocal<>();
local.set("我是主线程");
Log.d(TAG, "主线程: " + local.get());

new Thread(() -> {
    local.set("我是线程 1");
    Log.d(TAG, "线程 1: " + local.get());
}).start();

new Thread(() -> {
    Log.d(TAG, "线程 2: " + local.get());
}).start();
```

三条线程访问同一个 `ThreadLocal` 对象，看到的值分别是「我是主线程」「我是线程 1」和 `null`。两条子线程的日志先后取决于调度。这个例子对应 Looper 的保存方式。参考《Android 开发艺术探索》第 377 页。

## 一条消息怎样从发送走到执行

### 1. Handler 投递到队列

`sendMessage(msg)` 发送一条已有的消息。`post(runnable)` 先通过 `getPostMessage()` 把任务放进消息的 `callback` 字段，再走同一条发送路径。

```java
public final boolean post(Runnable r) {
    return sendMessageDelayed(getPostMessage(r), 0);
}
```

消息最终经过 `sendMessageAtTime()`，进入 Handler 保存的队列。入队前，Handler 把自己写到 `msg.target`。到达 `MessageQueue.enqueueMessage()` 后，队列在 `synchronized (this)` 内按执行时间插入链表。取消息和移除消息时也用同一把队列锁保护链表。

### 2. Looper 取出待处理的消息

运行 `Looper.loop()` 的线程调用 `queue.next()`。有到点的消息时，`next()` 从链表里取出一条并返回。Looper 根据 `msg.target` 找到接收它的 Handler，再调用 `dispatchMessage()`。

```java
Message msg = queue.next(); // 暂时没有可处理的消息时，这里可能等待。
// 省略退出条件和日志。
msg.target.dispatchMessage(msg);
```

### 3. Handler 执行回调

`dispatchMessage()` 依次检查下面三个分支：

1. 如果 `msg.callback` 有值，调用这个 `Runnable` 的 `run()`。这是 `post()` 走的路径。
2. 否则，如果构造 Handler 时传入了 `Handler.Callback`，调用它的 `handleMessage()`。它返回 `true` 时，分发结束。
3. 其余情况调用 Handler 自己的 `handleMessage()`。

分发和回调都是正在运行 Looper 的那条线程里的普通方法调用。从子线程投递到主线程时，线程之间通过队列交接消息。

相关源码是 `frameworks/base/core/java/android/os/` 下的 `Handler.java`、`Looper.java` 和 `MessageQueue.java`。

## 延时消息、主线程等待和 ANR

### 延时消息到点后怎样执行

`postDelayed(r, delayMillis)` 用 `SystemClock.uptimeMillis()` 加上延时得到消息的执行时间 `when`。队列按这个时间排序。如果下一条可处理的消息还没到点，`MessageQueue.next()` 算出 `when - now`，把剩余时间传给 `nativePollOnce()`。等待到期后，主线程重新检查队列，然后处理已到点的消息。

这个时间表示消息最早何时可以执行。即使 `post()` 的延时为零，主线程也要等当前回调返回，才有机会取下一条消息。

### Looper 等待时怎样唤醒

暂时没有可处理的消息时，主线程可以在 native 层的 `epoll_wait()` 中阻塞。Android 6.0.1 的 Looper 用 `eventfd` 作为主动唤醒的通知通道。如果队列正在等，而且新消息插到了队首，入队代码会调用 `nativeWake()`，让 Looper 往 `eventfd` 写入通知。等待返回后，主线程重新检查队列。同步屏障在队首时，插入首个可绕过屏障的异步消息也可能触发唤醒。

已经有延时消息时，等待还可以因为超时而返回。其他被监控的文件描述符就绪，也能让 `epoll_wait()` 返回。可以在 `frameworks/base/core/jni/android_os_MessageQueue.cpp` 和 `system/core/libutils/Looper.cpp` 对照这条路径。

**移除延时消息会唤醒主线程吗？** Android 6.0.1 的普通 `removeMessages()` 和 `removeCallbacks()` 通过修改队列取消待执行项，本身不会调用 `nativeWake()`。主线程在已有的等待结束，或被其他事件唤醒后，再检查更新过的队列。

### 主线程一直运行 Looper，为什么通常不会触发 ANR

Looper 循环让主线程能持续处理输入、生命周期回调和应用自己投递的任务。暂时没有工作时，主线程在内核里等事件或超时，CPU 可以去执行其他就绪任务。只有这个核心没有可运行的任务时，调度器才会选空闲任务。

输入 ANR 看的是输入能否及时得到处理。例如，如果主线程在 `onCreate()` 或点击回调里长时间执行耗时操作，后续输入便可能等到系统超时。Lab 2 的 D3a 就展示了同一个机制：任务已经投递，主线程仍在当前点击回调里，队列里的任务只能继续等。

## 同步消息、异步消息和同步屏障

普通 Handler 默认发送同步消息。创建 Handler 时如果设置了 `async`，它发送的消息会被标记为异步。两者都进入同一个队列，由同一条 Looper 线程处理。

当同步屏障到达队首，`MessageQueue.next()` 通过 `target == null` 识别屏障，向后查找异步消息。已到点的异步消息可以绕过屏障。屏障移除后，原来被挡住的同步消息可以继续分发。屏障可用于让界面绘制相关的消息及时处理。已经开始运行的回调仍会占用这条线程，异步消息也要等线程回到队列。

## IdleHandler 什么时候执行

当队列为空，或队首消息还没到执行时间时，`MessageQueue.next()` 可以调用已注册的 `IdleHandler.queueIdle()`。返回 `false` 会移除这个 IdleHandler。返回 `true` 会保留它，供之后空闲时再次调用。

```kotlin
Looper.myQueue().addIdleHandler {
    Log.i("HandlerActivity", "width ${btFive.width} height ${btFive.height}")
    false
}
```

主队列的 IdleHandler 仍在主线程上执行，适合短小、可以延后的工作，例如把部分非必要初始化移出启动回调。上面的例子可以用于观察控件宽高。需要等布局完成再读尺寸时，可以使用 `ViewTreeObserver.OnGlobalLayoutListener`。队列空闲和某个 View 完成布局是两个时机。

## 参考资料

- 消息队列和分发：[入队算法笔记](https://www.jianshu.com/p/9efe3b48b730)、[Handler 源码笔记](https://juejin.cn/post/6844904113470177293)、[视频讲解](https://www.bilibili.com/video/BV1VE411Z7Ay?p=4)、[郭霖的 Handler 文章](http://blog.csdn.net/guolin_blog/article/details/9991569)。
- 延时与唤醒：[延时消息源码](https://www.jianshu.com/p/68083d432b3f)、[知乎笔记](https://zhuanlan.zhihu.com/p/260661053)、[CSDN 笔记](https://blog.csdn.net/thh159/article/details/103644489)、[主线程循环与休眠](https://www.jianshu.com/p/a7969d73c120)。
- 同步屏障与空闲回调：[异步消息与同步屏障](https://blog.csdn.net/ly502541243/article/details/109091386)、[IdleHandler 笔记](https://blog.csdn.net/ZYJWR/article/details/103086664)、[IdleHandler 问答](https://www.wanandroid.com/wenda/show/8723)。
- 延伸阅读：[Handler 问题整理](https://juejin.cn/post/6844904150140977165)、[Handler 视频](https://www.bilibili.com/video/BV1A7411M7zQ)、[小专栏笔记](https://xiaozhuanlan.com/topic/0843791256)、[Glide 中的 Handler 用法](https://www.bilibili.com/video/BV1FU4y1V7HE)、[SegmentFault 文章](https://segmentfault.com/a/1190000039809784)。原笔记还提到可查看 LeakCanary 中的空闲回调用法。
