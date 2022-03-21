---
title: battery-research
date: 2022-03-13 20:39:05
tags: ANDROID
---



### 耗电提醒

<img src="https://static001.geekbang.org/resource/image/c6/1b/c6d2c20c09e84190c7b4a64578d0cc1b.png" alt="img" style="zoom:30%;" />



### 耗电问题难点

* 缺乏现场，无法复现。

  用户上传某个截图，你的应用耗电占比 30%。通过电量的详细使用情况，我们可能会有一些猜测。但是用户也无法给出更丰富的信息，以及具体是在什么场景发生的，可以说是毫无头绪。

  <img src="https://static001.geekbang.org/resource/image/7a/b2/7ae7234370738c60d2685c8b096a19b2.png" alt="img" style="zoom: 50%;" />

* 信息不全，难以定位。

  如果是开发人员或者厂商可以提供 bug report，利用 Battery Historian 可以得到非常全的耗电统计信息。但是 Battery Historian 缺失了最重要的堆栈信息，代码调用那么复杂，可能还有很多的第三方 SDK，我们根本不知道是哪一行代码申请了 WakeLock、使用了 Sensor、调用了网络等。

  

* 无法评估结果。

  通过猜测，我们可能会尝试一些解决方案。但是从 Android 4.4 开始，我们无法拿到应用的耗电信息。尽管我们解决了某个耗电问题，也很难去评估它是否已经生效，以及对用户产生的价值有多大。

* 信息不全，难以定位

  如果是开发人员或者厂商可以提供 bug report，利用 Battery Historian 可以得到非常全的耗电统计信息。但是 Battery Historian 缺失了最重要的堆栈信息，代码调用那么复杂，可能还有很多的第三方 SDK，我们根本不知道是哪一行代码申请了 WakeLock、使用了 Sensor、调用了网络等。

### 系统如何计算 App 电量

电量 = 功率 * 时间 = 电压 * 电流 * 时间

模块电量（mAh） = 模块电流（mA）* 模块耗时（h） 

App电量 = SUM(模块功率 * 模块时间)



#### 各模块的功率

![img](https://static001.geekbang.org/resource/image/b0/2b/b01e359b45d22bd80efda51eee2f5f2b.png)

BatteryStatusService：用来统计电量工作的，各模块的功率在 power_profile.xml 中，再加上各模块的使用耗时就能统计出电量了

https://android.googlesource.com/platform/frameworks/base/+/master/core/res/res/xml/power_profile.xml 

1. 功率：功率在 power_profile.xml 中

2.  时长：StopWatch 用来计算 App 各硬件模块的使用时长，怎么计算 Wifi 使用了多久？

WifiManager.startScan() --> WifiscanningServiceImp --> BatteryStatsImpl.noteWifiScanStartedFromSource(mScanWorkSource);

埋点

3.  计算：PowerCalculators ，每个硬件都有相应的命名对象， 主要用来计算电量

WifiPowerCalculator -> calculateApp

https://www.jianshu.com/p/672d008c4ad3

https://juejin.cn/post/6844904195523346439



### 系统限制


#### 提醒情况

从 Android 9.0 开始,，Google 对电源管理引入了几个更加严格的限制。

<img src="https://static001.geekbang.org/resource/image/13/8e/13697353748c1637643a6970db22808e.png" alt="img" style="zoom: 50%;" />

#### 应用分组

根据DeepMind调整应用待机分组，各个厂商可选择使用DeepMind的提供的模型.

* Active : App基本不会受到后台限制。

* Rare  : Jobs和Alarms都会受到延迟,访问网络的频率也会受到限制。
* Frequent rare ： 发送过量的高优先级FCM message, 系统会把这些信息降级为 普通优先级.

<img src="/Users/m/Documents/BLOG/source/_posts/battery/Screen Shot 2022-03-08 at 2.16.10 PM.png" alt="Screen Shot 2022-03-08 at 2.16.10 PM" style="zoom:50%;" />






### 后台限制

<img src="/Users/m/Documents/BLOG/source/_posts/battery-research/back_restrict.jpg" alt="back_restrict" style="zoom:30%;" />



1. Android Vitals 的规则 : Android P 是通过 Android Vitals 监控后台耗电

   https://developer.android.google.cn/topic/performance/vitals

Android Vitals 核心指标：

[ANR 发生率](https://developer.android.google.cn/topic/performance/vitals/anr)
[崩溃率](https://developer.android.google.cn/topic/performance/vitals/crash)
[唤醒次数过多](https://developer.android.google.cn/topic/performance/vitals/wakeup)
[部分唤醒锁定被卡住](https://developer.android.google.cn/topic/performance/vitals/wakelock)

其他指标：

[后台 Wi-Fi 扫描次数过多](https://developer.android.google.cn/topic/performance/vitals/bg-wifi)
[后台网络使用量过高](https://developer.android.google.cn/topic/performance/vitals/bg-network-usage)
[应用启动时间](https://developer.android.google.cn/topic/performance/vitals/launch-time)
[呈现速度缓慢](https://developer.android.google.cn/topic/performance/vitals/render)
[冻结的帧](https://developer.android.google.cn/topic/performance/vitals/frozen)
[权限遭拒](https://developer.android.google.cn/topic/performance/vitals/permissions)

2. 可参考规则

   <img src="https://static001.geekbang.org/resource/image/d4/be/d48b7e4d3fdceb101fa7716b5892b0be.png" alt="img" style="zoom:50%;" />

3. 华为公开过他们后台资源使用的“红线”

   <img src="https://static001.geekbang.org/resource/image/86/ff/86a65ea0d9216a11a341d7224fce93ff.png" alt="img" style="zoom:50%;" />



例如长时间获取 WakeLock、WiFi 和蓝牙的扫描等。为什么说耗电优化第一个方向就是优化应用后台耗电，因为大部分厂商预装项目要求最严格的正是应用后台待机耗电。

​	虽然上面的标准可能随时会改变，但是可以看到，Android 系统目前比较关心后台 Alarm 唤醒、后台网络、后台 WiFi 扫描以及部分长时间 WakeLock 阻止系统后台休眠。

https://blog.dreamtobe.cn/2016/08/15/android_scheduler_and_battery/





<img src="/Users/m/Documents/BLOG/source/_posts/battery-research/test_background_limig.jpg" alt="test_background_limig" style="zoom:50%;" />



### 后台操作原则

1. 减少 :  减少后台运行

2. 延迟  : 延迟到合适的时间，例如设备充上电

3. 合并: 合并后台操作

​	WorkManager : 后台操作的首选



### 耗电优化方式

1. 灭屏时停止动画 

   我们可以监听灭屏以及亮屏的广播，在灭屏的时候停止 surfaceView 的动画绘制。在亮屏的时候，恢复动画的绘制。
   
   

4. 监听手机充电状态

   这里我们就需要思考，根据具体的业务，考虑将一些不需要及时地和用户交互的操作放到充电 的时候去做。比如：360 手机助手，当充上电的时候，才会自动清理手机垃圾，自动备份上传图片、联系人 等到云端，从而避免当用户手机低电量时，任然继续进行耗电操作。

   ```java
   IntentFilter ifilter = new IntentFilter(Intent.ACTION_BATTERY_CHANGED);
   Intent batteryStatus = context.registerReceiver(null, ifilter);
   
   //获取用户是否在充电的状态或者已经充满电了
   int status = batteryStatus.getIntExtra(BatteryManager.EXTRA_STATUS, -1);
   boolean isCharging = status == BatteryManager.BATTERY_STATUS_CHARGING || status == BatteryManager.BATTERY_STATUS_FULL;
   ```

4. Sync Adapter

   用于同步服务端与本地设备中的数据。通常是用于同步较多的数据，如系统联系人信息、Dropbox等。

   
   
5.  找到需求场景的替代方案

   以推送为例，我们是否可以更多地利用厂商通道，或者定时的拉取最新消息这种模式。如果真是迫不得已，是不是可以使用 foreground service 或者引导用户加入白名单。后台任务的总体指导思想是减少、延迟和合并，可以参考微信一个小伙写的[《Android 后台调度任务与省电》](https://blog.dreamtobe.cn/2016/08/15/android_scheduler_and_battery/)。在后台运行某个任务之前，我们都需要经过下面的思考：<img src="https://static001.geekbang.org/resource/image/67/ac/67488fb06348423717cb0adba242bdac.png" alt="img" style="zoom: 33%;" />







#### 插桩

写一个基础类，然后在统一的调用接口中增加监控逻辑。

以 WakeLock 为例：

```java

public class WakelockMetrics {
    // Wakelock 申请
    public void acquire(PowerManager.WakeLock wakelock) {
        wakeLock.acquire();
        // 在这里增加Wakelock 申请监控逻辑
    }
    // Wakelock 释放
    public void release(PowerManager.WakeLock wakelock, int flags) {
        wakelock.release();
        // 在这里增加Wakelock 释放监控逻辑
    }
}
```



#### 电量监控工具

* Battery Historian

​		**无法评估结果**

* Energy Profiler      

* XHook(爱奇艺), 拿到所有线程的 cpu 耗时时间 。

* facebook Battery-Metrics

  它监控的数据非常全，包括 Alarm、WakeLock、Camera、CPU、Network 等，而且也有收集电量充电状态、电量水平等信息。Battery-Metrics 只是提供了一系列的基础类，在实际使用中，接入者可能需要修改大量的源码。但对于一些第三方 SDK 或者后续增加的代码，我们可能就不太能保证可以监控到了。这些场景也就无法监控了，所以 Facebook 内部是使用插桩来动态替换。

  Facebook 并没有开源它们内部的插桩具体实现方案，可以使用 ASM、Aspectj 这两种插桩方案了。

* matrix BatteryCanary

  
  
  ANR 内存使用 优化好了， 电量肯定没问题，
  
  如果电量不行，程序有问题
  
  
  
  
  
  
  
  





