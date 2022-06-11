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
