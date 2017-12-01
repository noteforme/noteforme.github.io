---
title: ACTIVITY
comments: true
date: 2017-08-11 10:14:11
tags:
categories: ANDROID

---

##Activity了解
An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(View).
Acitivty和用户交互，所以也是用的最多的

#1.想到一个问题:界面Ａ　到界面Ｂ，是Ａ的onStop()先执行　还是Ｂ的onResume()先走
 先用事实说话吧
  
     I/BaseActivity: MainActivity -- onCreate() -- 
     I/BaseActivity: MainActivity -- onStart() -- 
     I/BaseActivity: MainActivity -- onResume() -- 
     I/BaseActivity: MainActivity -- onPause() -- 
     I/BaseActivity: StopResumeActivity -- onCreate() -- 
     I/BaseActivity: StopResumeActivity -- onStart() -- 
     I/BaseActivity: StopResumeActivity -- onResume() -- 
     I/BaseActivity: MainActivity -- onStop() -- 
从打印结果可以看到StopResumeActivity.onResume()先执行,然后是MainActivity的onStop
先看看
onResume() : Called when the activity will start interacting with the user. At this point your activity is at the top of the activity stack, with user input going to it.

onStop() : Called when the activity is no longer visible to the user, because another activity has been resumed and is covering this one. This may happen either because a new activity is being started, an existing one is being brought in front of this one, or this one is being destroyed.
Followed by either onRestart() if this activity is coming back to interact with the user, or onDestroy() if this activity is going away.

大概知道原因，但是还是不够说服力，因为onStart()方法已经对用户可见了，为什么MainActivity -- onStop()不在 StopResumeActivity -- onStart()后面呢？
onStart：　Called when the activity is becoming visible to the user.
Followed by onResume() if the activity comes to the foreground, or onStop() if it becomes hidden.

参考：https://developer.android.com/reference/android/app/Activity.html

＃Ａctivity四种启动模式
- standard:这种方式总会为目标Ａctivity创建一个新实例，并將Activity添加当前Task中
- singleTop: 和 standard基本一致，如果将要被启动的Activity已经位于栈顶，系统不会创建目
　　　标Activity而是复用栈顶的Activity.
-  singleTask :在同一个Task只有一个实例，　如果要启动的Activity不存在，那么系统将会创建该实例   
　  如果将要启动的Activity不存在，那么系统将会创建一个全新的TASK,再创建目标Activity实例并放入新的Ｔask.
   如果将要启动的Ａctivity不在栈顶，系统会把位于该Activity上面的所有其他Activity全部移除Task,使该Activity位于栈顶
- singleInstance :全局单例
   如果要启动的Activity不存在，参考SingleTask
   如果将要启动的Activity已经存在，无论它位于哪个应用程序，哪个Task,系统都会把Activity所在的Task转到前台,使　Ａctivity显示.

http://blog.csdn.net/mynameishuangshuai/article/details/51491074