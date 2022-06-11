---
title: battery
date: 2022-02-23 13:50:10
tags: ANDROID
---



### 性能与功耗

https://github.com/JsonChao/Awesome-Android-Performance

https://developer.android.google.cn/topic/performance

#### battery history 

https://developer.android.com/topic/performance/power/setup-battery-historian?hl=zh-cn



```
 adb shell dumpsys batterystats > /Users/m/Documents/BATTELY/batterystats.txt
 
 adb bugreport /Users/m/Documents/BATTELY/bugreport.zip
```



指标查看

https://juejin.cn/post/6844903779268034574



### 三种情况

* 省电模式
* 应用待机分组
* 后台限制

这三种情况，开发者都需要对App进行测试，保证正常运行

https://www.bilibili.com/video/BV1Ut411d7U3/





<img src="/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.29.40 PM.png" alt="Screen Shot 2022-03-08 at 2.29.40 PM" style="zoom:50%;" />



#### 应用分组

<img src="/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.16.10 PM.png" alt="Screen Shot 2022-03-08 at 2.16.10 PM" style="zoom:50%;" />



#### 开发者保证无论哪个分组 App都没问题

1. 测试通知是否发送
2. Api接口： 获取运行时应用所在的分组



<img src="/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.17.59 PM.png" alt="Screen Shot 2022-03-08 at 2.17.59 PM" style="zoom:50%;" />

#### 省电模式

该模式App正常运行

![Screen Shot 2022-03-08 at 2.27.35 PM](/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.27.35 PM.png)





### 原则

减少 :  减少后台运行

延迟  : 延迟到合适的时间，例如设备充上电

合并: 尽量合并后台操作

WorkManager : 后台操作的首选



### 工具

#### Android Vitals

根据描述的不良行为来优化分组

https://developer.android.google.cn/topic/performance/vitals



#### Energy profiler

<img src="/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.36.21 PM.png" alt="Screen Shot 2022-03-08 at 2.36.21 PM" style="zoom:50%;" />









### 场景操作

#### 监听手机充电状态

这里我们就需要思考，根据具体的业务，考虑将一些不需要及时地和用户交互的操作放到充电 的时候去做。比如：360 手机助手，当充上电的时候，才会自动清理手机垃圾，自动备份上传图片、联系人 等到云端，从而避免当用户手机低电量时，任然继续进行耗电操作。



#### 屏幕唤醒 (继续研究我们App细节)

当 Android 设备空闲时，屏幕会变暗，然后关闭屏幕，最后会停止 CPU 的运行，这样可以防 止电池电量掉的快。但有些时候我们需要改变 Android 系统默认的这种状态:比如玩游戏时我 们需要保持屏幕常亮，比如一些下载操作不需要屏幕常亮但需要 CPU 一直运行直到任务完成。

保持屏幕常亮比较好的方式是在 Activity 中使用 FLAG_KEEP_SCREEN_ON 的 Flag。

![在这里插入图片描述](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/2/19/16904495558a8e3a~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



这个方法的好处是不像唤醒锁(wake locks)，需要一些特定的权限(permission)。并且能 正确管理不同 app 之间的切换，不用担心无用资源的释放问题。

另一个方式是在布局文件中使用 android:keepScreenOn 属性:

![在这里插入图片描述](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/2/19/16904495828e4789~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



android:keepScreenOn = “true”的作用和 FLAG_KEEP_SCREEN_ON 一样，使用代码的好 处是你允许你在需要的地方关闭屏幕。

注意:一般不需要人为的去掉 FLAG_KEEP_SCREEN_ON 的 flag，windowManager 会管理好程序进入 后台回到前台的的操作。如果确实需要手动清掉常亮的 flag，使用

![在这里插入图片描述](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/2/19/169044957b65ed32~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



所以这里我们需要根据自己的 APP 实际情况，根据业务来控制好是否保持屏幕常量。比如 APP 需要支持视频播放。那么在播放的界面需要控制好不熄屏，当退出播放时，当然就没有了 这个设置。



#### JobScheduler (现在应该用WorkManger)

自 Android 5.0 发布以来，JobScheduler 已成为执行后台工作的很好的方式，其工作方式有 利于用户在适当的时机执行正确的事情。应用可以在安排作业的同时允许系统基于内存、电源 和连接情况进行优化。JobSchedule 的宗旨就是把一些不是特别紧急的任务放到更合适的时机 批量处理。这样做有两个好处:

- 避免频繁的唤醒硬件模块，造成不必要的电量消耗。
- 避免在不合适的时间(例如低电量情况下、弱网络或者移动网络情况下的)执行过多的 任务消耗电量。



#### GPS

Android 系统支持多个 Location Provider:

- **GPS_PROVIDER:** GPS 定位，利用 GPS 芯片通过卫星获得自己的位置信息。定位精准度高，一般在 10 米左右， 耗电量大;但是在室内，GPS 定位基本没用。
- **NETWORK_PROVIDER:** 网络定位，利用手机基站和 WIFI 节点的地址来大致定位位置，这种定位方式取决于服务器， 即取决于将基站或 WIF 节点信息翻译成位置信息的服务器的能力。
- **PASSIVE_PROVIDER:** 被动定位，就是用现成的，当其他应用使用定位更新了定位信息，系统会保存下来，该应用接 收到消息后直接读取就可以了。

如果 App 只是需要一个粗略的定位那么就不需要使用 GPS 进行定位，既耗费电量，定位的耗 时也久。



**及时注销定位监听** (怎么注销定位)


链接：https://juejin.cn/post/6844903779268034574



电量分析

https://developer.android.google.cn/topic/performance/vitals

https://www.youtube.com/watch?v=dx6LBaFqEHU&t=3s

https://tech.meituan.com/2018/03/11/dianping-shortvideo-battery-testcase.html

https://www.jianshu.com/p/3dc2ea40ed5a









docker -- run -p 1234:9999 gcr.io/android-battery-historian/stable:3.0 --port 9999



### YouTuBe Video

#### Battery Historian

Battery Historian Part 1: Basics and Installation Using Docker Tool-box

https://www.youtube.com/watch?v=6NQwtj0GZRo

Battery Historian Part 2: Steps to Collect BatteryStat logs and Creating Report on Console

https://www.youtube.com/watch?v=ZTzdrvYO8sM

Battery Historian Part 3: Report Analysis and Error Checks

https://www.youtube.com/watch?v=d_VFXzBhuuc



#### Battery Historian Part

https://www.youtube.com/watch?v=11aJfiqqBAk

https://www.youtube.com/watch?v=PK2VBuMMkn8

https://www.youtube.com/watch?v=6LuPsbwH3HE

https://www.youtube.com/watch?v=-v4NL5oqBq0





#### 耗电优化建议（实用）

https://juejin.cn/post/7034751791866576904

https://github.com/JsonChao/Awesome-Android-Notebook/blob/master/notes/Android%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.md#APP%E7%94%B5%E9%87%8F%E4%BC%98%E5%8C%96

https://www.bilibili.com/video/BV1Ut411d7U3/





#### 具体处理

主要是优化后台：Alarm 一定时间内的启动次数，WakeLock 一定时间内的时间/次数，Sensor，Wifi Scans ，Bluetooth , GPS，Camera，NetWork，CPU ，退到后台监控次数和时间，定义一个阀值超过提醒输出

其次是监控优化前台：监控线程 Cpu 时间，Gpu 绘制，拿到所有线程的 cpu 耗时时间 ANR dump

微信：优化，异步加载（比较长）1 分钟，io 读取 ANR （子线程）极端情况下死循环

 



耗电量是怎么计算的？

如果 app 需要监控应该怎么监控？

怎么监控异常耗电并且输出方便排查的信息？

 

### 监控工具

#### wechat **[matrix](https://github.com/Tencent/matrix)**

https://kaedea.com/2021/08/14/android-apm-battery-stats-opt/

https://blog.csdn.net/chuyouyinghe/article/details/117223373

 

分享做业务怎么脱颖而出？道理都是一样，最后进来的，项目基本完工了，不考核的三流业务，绩效业务。

尽最大的努力去支持他，1200条+，邮箱 WXG ，推荐了我，尽可能积极乐观主动。

熬，低调厚积薄发，做别人不愿意做的事情，摆正好心态



http://www.moyck.com/articles/2019/10/14/1571046711964.html

http://www.woshipm.com/it/855066.html

https://xiaojianchen.gitbook.io/performance-optimization/android-xing-neng-you-hua-zhi-dian-liang

https://juejin.cn/post/6844904195523346439



https://www.bilibili.com/video/BV1st411N7cC?p=5
