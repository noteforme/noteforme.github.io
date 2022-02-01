---
title: PerformancePatterns
comments: true
date: 2017-08-16 09:56:08
tags:
categories: ANDROID
---
 性能优化



## 布局优化

https://www.jianshu.com/p/ee9e4b8cb95f



##   ANR

  全称 Application Nor Responding,在debug的时候经常会出现的一个框框，询问关闭或等待，在正常环境下出现的情况
  *　主线程IO 操作
  *　主线程耗时任务
  *　主线程错误的操作 Thread.wait或Thread.sleep
    Android系统监控程序的响应状况，出现下面两种情况弹出ANR对话框
    *应用在5秒内未响应用户的输入事件(按键或者触摸)
    *BroadcastReceiver未在　10秒内完成相关的处理

##   AndroidStudio Profiler工具

[这个视频](https://www.youtube.com/watch?v=qJ1QLgiL24o) 讲解了找出内存泄漏的步骤　
还有AndroidStudio3.0的[图文方法](https://www.jianshu.com/p/bdfd2a6b2681) 

##　leakcanary

https://github.com/square/leakcanary

图片处理

https://developer.android.com/topic/performance/graphics/index.html
https://developer.android.com/topic/performance/graphics/load-bitmap.html#read-bitmap



* apk size

  https://mp.weixin.qq.com/s/jnZpgaRFQT5ULk9tHWMAGg

https://www.jianshu.com/p/fba7b43bdc9c

性能优化

https://mp.weixin.qq.com/s/ceXsH06fUFa7y4lzi4uXzw

https://mp.weixin.qq.com/s/xqrGukIqA2zpGsGzah2EzA



包体积优化

https://juejin.im/post/6854573210408189960