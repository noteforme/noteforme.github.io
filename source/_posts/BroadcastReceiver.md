---
title: BroadcastReceiver
comments: true
date: 2017-08-12 15:44:53
tags:
categories:  ANDROID

---
## 介绍
　广播使应用程序间传输传输信息的机制，主要用来监听系统或者应用发出的广播信息。
 
# BroadcastReceiver使用注意
   当系统或应用发出广播时，扫描系统所有广播接收者(无论时静态注册还是动态注册方式)，通过action匹配將广播发给相应接收者，接收者收到
   广播后会产生一个广播实例，执行onReceiver()，这个实例生命周期只有10秒,10秒内没执行结束onReceiver，系统会报错,所以不能再onReceiver()执行耗时操作,onReceiver结束后，实例被销毁.

#BroadcastReceiver注册
 优两种方式
 - 静态注册 (推荐使用)
   在AndroidManifest.xml设置receiver并接收action在应用·安装应用后无论是否处于运行状态，广播接收器已经常驻系统中，要销毁静态注册广播接收者，可以通过PackageManager將Ｒeceiver禁用.
 - 动态注册方式
 　　 @Override
	   protected void onResume() {
            // 动态注册广播 (代码执行到这才会开始监听广播消息，并对广播消息作为相应的处理)
            receiver = new MyBroadcastReceiver();
            IntentFilter intentFilter = new IntentFilter("android.provider.Telephony.SMS_RECEIVED" );
            registerReceiver( receiver , intentFilter);

            super.onResume();
	   }

#区别
 
 1.静态注册的广播接收者已经安装旧常驻在系统中，不需要重新启动唤醒接收者
  在lern注册了MyBroadcastReceiver,然后在Rxlean中发送广播也可以看到接收到的数据
  
  　　　2715/com.hyhy.lern I/MyBroadcastReceiver:   输出数据  I am RXleran 
   [广播代码](https://github.com/BlogForMe/AndroidDemo/blob/master/lern/src/main/java/com/hyhy/lern/broadcast/MyBroadcastReceiver.java)
   [发送代码](https://github.com/BlogForMe/AndroidDemo/blob/master/RXleran/src/main/java/com/hyhy/rxleran/MainActivity.java)
 2.动态注册的广播接收者随应用的生命周期,由registerReceiver开始监听，由unregitsterReceiver撤销监听,如果应用退出后没有撤销已经注册的监听者将会报错。
   
  没有撤销的警告:   MyBroadcastReceiver@425e0890 that was originally registered here. Are you missing a call to unregisterReceiver()?
   
 3.当广播接收者通过intent启动activity或service，如果intent中无法匹配到相应的组件，动态注册的广播接收者将会导致应用报错,而静态注册的广播接收者不会由任何报错，因为应用安装后广播接收者已经和应用脱离了关系。

参考：https://developer.android.com/guide/components/broadcasts.html

