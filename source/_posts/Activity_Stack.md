---
title: Activity_Stack
comments: true
date: 2021-01-16 17:49:32
tags:
categories: ANDROID
---

https://developer.android.com/reference/android/app/Activity.html

https://developer.android.com/guide/components/activities/tasks-and-back-stack.html

#### Activity

An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(View).
Acitivty和用户交互，所以也是用的最多的

![](https://developer.android.google.cn/images/activity_lifecycle.png) 

##### lifecycle

1. onCreate():  Activity创建的时候调用，绑定数据。
2. onStart() : 当Activity对用户变得可见的时候调用.
3. onResume() : activity开始和用户交互的时候调用，这时候activity处于栈顶，伴随着用户的输入.
4. onPause() : 当activity失去前台状态，开始进入stopped/hidden or destroyed状态，不能再获取焦点，此时activity对用户可见，但是更新的UI的操作要快。
5. onStop()  :  当activity不再可见，可能是新的activity来到栈顶，或者当前activity正在被destroy.
6. onDestory(): 当前activity正在离开。



#####  想到一个问题:界面Ａ到界面Ｂ，是Ａ的onStop()先执行　还是Ｂ的onResume()先走

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

onStop() : 
大概知道原因，但是还是不够说服力，因为onStart()方法已经对用户可见了，为什么MainActivity -- onStop()不在 StopResumeActivity -- onStart()后面呢？

可能是onResume()调用后，新activity位于栈顶，之前的再onStop()



https://blog.csdn.net/c6E5UlI1N/article/details/119549972



#### OnNewIntent()

onNewIntent

added in [API level 1](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)

```
void onNewIntent (Intent intent)
```

if the Activity was already created and a new Intent is being delivered to `onNewIntent(android.content.Intent)`

This is called for activities that set launchMode to "singleTop" in their package, or if a client used the `FLAG_ACTIVITY_SINGLE_TOP` flag when calling `startActivity(Intent)`. In either case, when the activity is re-launched while at the top of the activity stack instead of a new instance of the activity being started, onNewIntent() will be called on the existing instance with the Intent that was used to re-launch it.

An activity will always be paused before receiving a new intent, so you can count on `onResume()` being called after this method.

Note that `getIntent()` still returns the original Intent. You can use `setIntent(Intent)` to update it to this new Intent.

| Parameters |                                                             |
| ---------- | ----------------------------------------------------------- |
| `intent`   | `Intent`: The new intent that was started for the activity. |

这个方法用于singleTop  singleTask两种启动模式

先看看  singleTask方式：FirstActivity  

```
<activity android:name=".launchmode.FirstActivity"
            android:launchMode="singleTask"/>
```

​		

   从 FirstActivity  --> LaunchActivity --> FirstActivity 生命周期方法



>  04-24 11:49:14.602 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onCreate()
>  04-24 11:49:14.672 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onStart()
>  04-24 11:49:14.679 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onResume()
>  04-24 11:49:32.085 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onPause()
>  04-24 11:49:32.099 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onCreate()
>  04-24 11:49:32.127 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onStart()
>  04-24 11:49:32.132 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onResume()
>  04-24 11:49:32.462 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onStop()
>  04-24 11:49:53.681 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onPause()
>  04-24 11:49:53.701 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode:  onNewIntent(Intent intent)
>  04-24 11:49:53.702 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onStart()
>  04-24 11:49:53.703 26674-26674/com.jonzhou.mineutils D/FirstActivity LaunchMode: onResume()
>  04-24 11:49:54.049 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onStop()
>  04-24 11:49:54.049 26674-26674/com.jonzhou.mineutils D/LaunchActivity LaunchMode: onDestroy()



然后可以通过 onNewIntent(Intent intent);获取传回来的数据

```
        String data1 = intent.getStringExtra(newIntent);
        String data2 = getIntent().getStringExtra(newIntent);  //这种方式获取不到
        setIntent(intent);                                     //通过这种设置获取
        String data3 = getIntent().getStringExtra(newIntent);
        Timber.d("onNewIntent  " + data3);
```



为什么要设置 setIntent(intent)

> 我们在多次启动同一个栈唯一模式下的activity时，在onNewIntent()里面的getIntent()得到的intent感觉都是第一次的那个数据。对，这里就是这个陷阱。因为它就是会返回第一个intent的数据

https://blog.csdn.net/qq_16628781/article/details/51539715



#### ActivityStack

TaskRecord不同的应用就不同的 任务栈



安卓系统管理着不同模式下的多个ActivityStack（比如在home launcher界面需要有一个ActivityStack，画中画模式，分屏模式等），一个ActivityStack可以包含很多个TaskRecord，一个TaskRecord又可以包含很多个ActivityRecord。

每一个ActivityRecord都会有一个Activity与之对应，一个Activity可能会有多个ActivityRecord，因为Activity可以被多次实例化，取决于其launchmode。一系列相关的ActivityRecord组成了一个TaskRecord，TaskRecord是存在于ActivityStack中，ActivityStackSupervisor是用来管理这些ActivityStack的。



![](https://img-blog.csdnimg.cn/img_convert/c193e7342fb1015f2f18269b27382f06.png)







#### taskAffinity

清单文件中Application和Activity标签都可以使用android:taskAffinity，值为String类型。

它代表这个Activity所希望归属的Task，在默认情况下，同一个app中的所有Activity拥有共同的Affinity，即manifest中定义的package。

另外task的affinity取决于它的根部Activity。

![](https://img-blog.csdnimg.cn/img_convert/732fa07cf455afaf801593830285e9e9.png)

如果使用SingleInstance模式启动的Activity，如果没有指定affinity的话，创建的新task的affinity还是app的包名。



##### FLAG_ACTIVITY_NEW_TASK

在默认情况下，目标Activity将与startActivity的调用者处于同一task中。但如果用户特别指定了FLAG_ACTIVITY_NEW_TASK，表明它希望为Activity重新开设一个Task。这时就有两种情况：

1. 假如当前已经有一个Task，它的affinity与新Activity是一样的，那么系统会直接用此Task来完成操作，而不是另外创建一个Task；
2. 否则系统需要创建一个Task。



#### allowTaskReparenting 什么意思



#### Activity launch mode

启动模式

1. Androidmanifest 设置 launch mode是对目标Activity而言

2. Intent flag比 Androidmanifest的优先级更高





##### 4种启动模式

我理解的启动模式，针对的是被启动的Activity

* standard 

  在不同的Task中打开同一个Activity，Activity会被创建多个实例。分别放进打开它的Task中。

  例如: 点开短信中的电话号码， 新的 打电话的Activity会存在于   短信Task中。

* singleTop

  

* singleTask

  > The system creates a new task and instantiates the activity at the root of the new task. However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its `onNewIntent()` method, rather than creating a new instance. Only one instance of the activity can exist at a time.
  >
  > 只会在一个Task出现,这个Task里只有一个这个Activity，全局唯一

* singleInstance 

  > Same as `"singleTask"`, except that the system doesn't launch any other activities into the task holding the instance. The activity is always the single and only member of its task; any activities started by this one open in a separate task.
  >
  > 除了全局唯一，还会独占一个Task



#### taskAffinity

每个Activity都有一个taskAffinity，默认取自Activity所在的Application的taskAffinity,而后者又默认取自app包名.

每个Task也有它自己的taskAffinity，它取自栈底的Activity的taskAffinity

默认情况下，Activity会直接进入当前的Task

但对于设置了  launchMode = "singleTask"的Activity,系统会先比对Activity和当前Task的taskAffinity是否相同

* 如果相同，依然正常入栈.

* 如果不同，新Activity会去寻找和它 taskAffinity相同的Task后入栈。

  如果找不到，系统就为它创建一个新的Task,或者创建一个新的Task.



MainActivity启动FirstActivity

```xml
<activity
    android:name=".component.launchmode.FirstActivity"
    android:taskAffinity="com.comm.mytask" />
```




       TaskRecord{dff3f0b #4947 A=com.comm.util U=0 StackId=1 sz=2}
             Run #4: ActivityRecord{e52ebf7 u0 com.comm.util/.component.launchmode.FirstActivity t4947}
             Run #3: ActivityRecord{cd6b037 u0 com.comm.util/.MainActivity t4947}



```xml
<activity
    android:name=".component.launchmode.FirstActivity"
    android:launchMode="singleTask"
    android:taskAffinity="com.comm.mytask" />
```

```
 	TaskRecord{3713c1e #4946 A=com.comm.mytask U=0 StackId=1 sz=1}
        Run #4: ActivityRecord{3d3bd58 u0 com.comm.util/.component.launchmode.FirstActivity t4946}
      TaskRecord{273cacc #4945 A=com.comm.util U=0 StackId=1 sz=1}
        Run #3: ActivityRecord{77277e0 u0 com.comm.util/.MainActivity t4945}
```

 添加完   `android:launchMode="singleTask"`后 多了个task	 com.comm.mytask



##### TaskAffinity和最近任务列表



```
adb shell dumpsys activity activities | sed -En -e '/Running activities/,/Run #0/p'
```



##### SingleInstance 不设置taskAffinity

SingleInstance会创建一个新的任务栈

```xml
<activity android:name=".component.launchmode.FirstActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity
    android:name=".component.launchmode.SecondActivity"
    android:launchMode="singleInstance" />

<activity android:name=".component.launchmode.ThirdActivity" />
```

 ```
 D/FirstActivity LaunchModeActivity: onCreate() taskId  86
 D/FirstActivity LaunchModeActivity: onStart()
 D/FirstActivity LaunchModeActivity: onResume()
 D/FirstActivity LaunchModeActivity: onPause()
 D/SecondActivity LaunchModeActivity: onCreate() taskId  85
 D/SecondActivity LaunchModeActivity: onStart()
 D/SecondActivity LaunchModeActivity: onRestoreInstanceState()
 D/SecondActivity LaunchModeActivity:  onNewIntent(Intent intent)
 D/SecondActivity LaunchModeActivity: onResume()
 D/FirstActivity LaunchModeActivity: onSaveInstanceState()
 D/FirstActivity LaunchModeActivity: onStop()
 D/SecondActivity LaunchModeActivity: onPause()
 D/ThirdActivity LaunchModeActivity: onCreate() taskId  86
 D/ThirdActivity LaunchModeActivity: onStart()
 D/ThirdActivity LaunchModeActivity: onResume()
 D/SecondActivity LaunchModeActivity: onSaveInstanceState()
 D/SecondActivity LaunchModeActivity: onStop()
 ```

可以看到 FirstActivity 、ThirdActivity在同一个栈中，SecondActivity单独在一个栈中

所以按返回键盘，先到 FirstActivity，然后到SecondActivity .

##### 栈内Activity查看，设置taskAffinity

```xml
     <activity android:name=".component.launchmode.SerachActivity" />
        <activity
            android:name=".component.onactivityresult.SecondActivity"
            android:launchMode="singleTask"
            android:taskAffinity="" />
        <activity
            android:name=".component.launchmode.FirstActivity"
            android:configChanges="orientation"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

> adb shell dumpsys activity

![](Activity_Stack/2021-01-23_3.31.08.png)



* taskAffinity属性的值为字符串，且中间必须含有分隔符"."
* standard模式，taskAffinity继承自Application的taskAffinity，而Application默认taskAffinity为包名，所以MainActivity的taskAffinity为包名。

https://developer.android.com/guide/components/activities/tasks-and-back-stack

https://blog.csdn.net/mynameishuangshuai/article/details/51491074

https://blog.csdn.net/zhangjg_blog/article/details/10923643



https://blog.csdn.net/javazejian/article/details/52072131





生命周期视图

http://yhz61010.iteye.com/blog/2389877

https://blog.csdn.net/qq_16628781/article/details/51539715





#### 然而 ANDROID 4.4 启动模式会出现问题


http://www.jianshu.com/p/2a9fcf3c11e4

http://blog.csdn.net/mynameishuangshuai/article/details/51491074

https://www.youtube.com/watch?v=r4T9zkhpmII





##### 任务栈一



 从图中我们看出前台任务栈分别为AB两个Activity，后台任务栈分别为CD两个任务栈，而且其启动模式均为singleTask，此时我们先启动CD，然后再启动AB，再有B启动D，此时后台任务栈便会被切换到前台，而且这个时候整个后退列表就变成了ABCD，请注意我们这里强调的是后退列表，而非栈合并。因此当用户点击back键时，列表中的Activity会依次按DCBA顺序出栈，如下图所示：

前面我们在分析singleTask模式时，提到过singleTask模式有些比较特殊的场景，现在我们就来了解了解它们。
特殊情景一:现在我们假设有如下两个Task栈,分别为前台任务栈和后台任务栈

  从图中我们看出前台任务栈分别为AB两个Activity，后台任务栈分别为CD两个任务栈，而且其启动模式均为singleTask，此时我们先启动CD，然后再启动AB，再有B启动D，此时后台任务栈便会被切换到前台，而且这个时候整个后退列表就变成了ABCD，请注意我们这里强调的是后退列表，而非栈合并。因此当用户点击back键时，列表中的Activity会依次按DCBA顺序出栈，如下图所示：

  这里我们通过两个应用ActivityTask和ActivityTask2来测试重现这个现象。因为两个是不同的应用所以启动时所在的栈也是不同。我们先启动ActivityTask2的应用，其ActivityC和ActivityD都是singleTask模式，然后再启动应用ActivityTask，此时ActivityC和ActivityD所在任务栈会被退居后台，而打开的ActivityA和ActivityB会在前台，而且都是默认模式。我们通过 adb shell dumpsys activity activities 命令查看此时栈的情况：

  我们可以看到由两个栈，分别为id=222且栈名为“com.cmcm.activitytask”的任务栈其包含ActivityA和ActivityB（下面简称AB，栈名一般默认和包名相同），另外一个任务栈，id=221，栈名为“com.cmcm.activitytask2”，其包含ActivityC和ActivityD（下面检测CD）。现在我们通过ActivityB去启动ActivityD，然后按back键回退

  我们可以看到包含CD的任务栈被提前的，虽然CD隔开了，但是我们从id和栈名可以发现他们是同一个栈，而AB所在的栈则在CD所在栈的后面，所以此时我们按back回退时，退出顺序是这样的D->C->B->A，动态图如下：



<img src="Activity_Stack/play.gif" alt="play" style="zoom:20%;" />



 到此我们对SingleTask模式又有了更深入的理解，但是我们发现上面的例子使用的是两个应用，所以才会有不同的任务栈，那么我们能不能在一个应用中存在多个不同的任务栈呢（暂时不考虑singleInstance 模式）？答案当然是肯定的啦，这就需要通过taskAffinity属性来设置不同的任务栈名称，不过这点将放在下篇来记录，本篇就先到这里告一段落哈。

https://blog.csdn.net/javazejian/article/details/52071885



##### 任务栈二

这篇文章讲的很好

https://blog.csdn.net/javazejian/article/details/52072131



1. TaskAffinity与singleTask应用场景

​		这个和allowTaskReparenting没关系

  假如现在有这么一个需求,我们的客户端app正处于后台运行，此时我们因为某些需要，让微信调用自己客户端app的某个页面，用户完成相关操作后，我们不做任何处理，按下回退或者当前Activity.finish()，页面都会停留在自己的客户端（此时我们的app回退栈不为空），这显然不符合逻辑的，用户体验也是相当出问题的。我们要求是，回退必须回到微信客户端,而且要保证不杀死自己的app.这时候我们的处理方案就是，设置当前被调起Activity的属性为：

LaunchMode=""SingleTask" taskAffinity="com.tencent.mm"

其中com.tencent.mm是借助于工具找到的微信包名，就是把自己的Activity放到微信默认的Task栈里面，这样回退时就会遵循“Task只要有Activity一定从本Task剩余Activity回退”的原则，不会回到自己的客户端；而且也不会影响自己客户端本来的Activity和Task逻辑。

2. TaskAffinity与allowTaskReparenting应用场景
     一个e-mail应用消息包含一个网页链接，点击这个链接将出发一个activity来显示这个页面，虽然这个activity是浏览器应用定义的，但是activity由于e-mail应用程序加载的，所以在这个时候该activity也属于e-mail这个task。如果e-mail应用切换到后台，浏览器在下次打开时由于allowTaskReparenting值为true，此时浏览器就会显示该activity而不显示浏览器主界面，同时actvity也将从e-mail的任务栈迁移到浏览器的任务栈，下次打开e-买了时并不会再显示该activity
        到此TaskAffinity就全部介绍完了，最后我们再来了解几个跟任务栈相关的属性参数；







