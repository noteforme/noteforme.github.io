---
title: PerformanceANR
date: 2022-06-07 14:41:30
tags: Performance
---



ANR



https://play.google.com/console/u/0/developers/5313279702199603526/app/4975846927718251228/vitals/crashes?installedFrom=PLAY_STORE&days=30

https://developer.android.com/topic/performance/vitals/anr.html



#####   ANR

  全称 Application Nor Responding,在debug的时候经常会出现的一个框框，询问关闭或等待，在正常环境下出现的情况

  * 主线程IO 操作

  * 主线程耗时任务

  * 主线程错误的操作 Thread.wait或Thread.sleep
    Android系统监控程序的响应状况，出现下面两种情况弹出ANR对话框
    *应用在5秒内未响应用户的输入事件(按键或者触摸)
    *BroadcastReceiver未在　10秒内完成相关的处理





Android Vitals 将连续丢帧超过 700 毫秒定义为冻帧，也就是连续丢帧 42 帧以上

https://time.geekbang.org/column/article/72642





卡顿优化

https://www.bilibili.com/video/BV1T44y1u7g6?spm_id_from=333.337.search-card.all.click

Block canary



https://blog.csdn.net/JArchie520/article/details/106710663

这里有ANR场景模拟





`ANR`产生原因和类型有以下几种：

#### 1、`Activity`在5秒钟之内无法响应屏幕触摸事件挥着键盘输入事件就会产生`ANR`。

KeyDispatchTimeout
 Reason：Input event dispatching timed out

#### 2、`BroadcastReceiver`在10秒钟之内还未执行完成就会产生`ANR`。

BroadcastTimeout
 Reason：Timeout of broadcast BroadcastRecord

#### 3、`Service`各个生命周期在20秒钟之内没有执行完成就会产生`ANR`。

ServiceTimeout
 Reason：Timeout executing service

#### 4、`ContentProvider`在10秒钟之内没有执行完成就会产生`ANR`。

ContentProviderTimeout
 Reason：timeout publishing content providers

在以上这几种原因中出现最多的一般是第一种，而且往往都是因为在写代码时不注意，在主线程做了耗时的操作。


链接：https://juejin.cn/post/6844904069731975176



各主流方案

##### BlockCanary

 主要是检测主线程的耗时操作，后面会写卡顿监控，主要基于Handler的打印

某些场景监控不到, 

##### ANR-WatcheDog

主要是往主线程插入一条监控消息， 隔一段时间来引爆这个炸弹,今天手写

轮询时间不好定呀,时间5s, 5s - 10s的有可能监控不到

##### SafeLooer

去代理了主线程Looper，这个不讲

##### FileObserver 

高版本没有权限,所以用不了











