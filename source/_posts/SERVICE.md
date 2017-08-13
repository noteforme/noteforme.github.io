---
title: SERVICE
comments: true
date: 2017-08-11 18:21:06
tags:
categories: ANDROID

---
 Service使一个应用程序组件，再后台能执行一些耗时的操作，并且不提供用户界面，由于无法再不同的Activity对同一Thread进行控制 ，此时就要考虑服务
 
 启动服务两种方式
- startService
 一个Service被startService多次启动,那么onCreate只会调用一次,onstart()会被调用多次, StartServcie:一旦启动，服务即可在后台无限期运行，即使启动服务的Ａctivity已被销毁也不受影响,直到调用stipService，或自身的stopSelf方法。内存不足系统也会结束服务。
 两种调用方法[代码](https://github.com/BlogForMe/AndroidDemo/blob/master/lern/src/main/java/com/hyhy/lern/ServiceActivity.java)
 
 ![Servcie lifeccle](https://developer.android.google.cn/images/service_lifecycle.png)

- bindService
  客户端通过IBinder接口于服务进行通信,客户端可以调用unbindServie关闭连接,当所有客户端关闭连接后系统会销毁服务.
  启动Log日志
  I/LearnService:  -- onCreate() --
  I/LearnService:  -- onBind() --
　一个Service被某个Activity调用Context.bindService绑定启动，不管bindService调用几次，onCreate方法只会调用一次,onStart不会调用.
 
- 还有一种情况是 先startService创建服务，然后bindService绑定，这时候调用stopService或stopSelf(),不会停止，只能使unbindService()停止服务

# IntentService
   通过HandlerThread单独开启一个线程一次处理所有的任务
   不需要调用stopSelft()关闭服务，任务结束后自动关闭
 08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onCreate() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task2
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.617 30532-30532/com.hyhy.lern I/MyIntentService: -- onDestroy() --

