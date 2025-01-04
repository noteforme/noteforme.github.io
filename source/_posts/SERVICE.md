---
title: SERVICE
comments: true
date: 2017-08-11 18:21:06
tags:
categories: ANDROID

---


# Service



https://developer.android.com/guide/components/services

https://developer.android.com/reference/android/app/Service



##### Service启动服务两种方式

*  startService 
*  bindService

##### startService

一个Service被startService多次启动,那么onCreate只会调用一次,onstart()会被调用多次, StartServcie:一旦启动，服务即可在后台无限期运行，即使启动服务的Ａctivity已被销毁也不受影响,直到调用stipService，或自身的stopSelf方法。内存不足系统也会结束服务。




 ![Servcie lifeccle](https://developer.android.google.cn/images/service_lifecycle.png)

##### bindService

客户端通过IBinder接口于服务进行通信,客户端可以调用unbindServie关闭连接,当所有客户端关闭连接后系统会销毁服务.
启动Log日志
I/LearnService:  -- onCreate() --
I/LearnService:  -- onBind() --
　一个Service被某个Activity调用Context.bindService绑定启动，不管bindService调用几次，onCreate方法只会调用一次,onStart不会调用.

1. service

   ```java
   public class BluetoothService extends Service {
   
   
       @Override
       public void onCreate() {
           super.onCreate();
           Timber.i(" onCreate()");
       }
   
       @Override
       public int onStartCommand(Intent intent, int flags, int startId) {
           Timber.i(" onStartCommand()");
           String param = intent.getStringExtra(PARRAM);
           Timber.i("接受    " + param);
           return super.onStartCommand(intent, flags, startId);
       }
   
       @Override
       public void onDestroy() {
           super.onDestroy();
           Timber.i(" onDestroy()");
       }
   
       @Override
       public void onLowMemory() {
           super.onLowMemory();
           Timber.i("onLowMemory()");
       }
   
       @Override
       public boolean onUnbind(Intent intent) {
           Timber.i("onUnbind(intent)()");
           return super.onUnbind(intent);
       }
   
       @Override
       public void onRebind(Intent intent) {
           super.onRebind(intent);
           Timber.i("onRebind(intent)()");
       }
   
   
       public class LocalBinder extends Binder {
           public BluetoothService getService() {
               return BluetoothService.this;
           }
       }
   
       private final IBinder mBinder = new LocalBinder();
   
       @Nullable
       @Override
       public IBinder onBind(Intent intent) {
           Timber.i("onBind(intent)()");
   
           return mBinder;
       }
   }
   
   ```

2. 启动

3. `bindService(intent,mServiceConnection,BIND_AUTO_CREATE);`

3. 回调

```java
  ServiceConnection mServiceConnection = new ServiceConnection() {
         @Override
         public void onServiceConnected(ComponentName name, IBinder service) {
             mBlueService =  ((BluetoothService.LocalBinder)service).getService();
             Timber.i("onServiceConnected(ComponentName name, IBinder service)");
         }

         @Override
         public void onServiceDisconnected(ComponentName name) {
            Timber.i("onServiceDisconnected(ComponentName name)");
         }
     };
```





-  先startService创建服务，然后bindService绑定，这时候调用stopService或stopSelf(),不会停止，只能使unbindService()停止服务

###### IntentService
   通过HandlerThread单独开启一个线程一次处理所有的任务
   不需要调用stopSelft()关闭服务，任务结束后自动关闭

```
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onCreate() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task2
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.617 30532-30532/com.hyhy.lern I/MyIntentService: -- onDestroy() --
```

https://developer.android.com/training/run-background-service/create-service.html



##### onStartCommand()

**1. onStartCommand方式中，返回START_STICKY**

首先我们来看看onStartCommand都可以返回哪些值：

调用Context.startService方式启动Service时，如果Android面临内存匮乏，可能会销毁当前运行的Service，待内存充足时可以重建Service。而Service被Android系统强制销毁并再次重建的行为依赖于Service的onStartCommand()方法的返回值。

**START_NOT_STICKY** 
 如果返回START_NOT_STICKY，表示当Service运行的进程被Android系统强制杀掉之后，不会重新创建该Service。当然如果在其被杀掉之后一段时间又调用了startService，那么该Service又将被实例化。那什么情境下返回该值比较恰当呢？  
 如果我们某个Service执行的工作被中断几次无关紧要或者对Android内存紧张的情况下需要被杀掉且不会立即重新创建这种行为也可接受，那么我们便可将 onStartCommand的返回值设置为START_NOT_STICKY。 
 举个例子，某个Service需要定时从服务器获取最新数据：通过一个定时器每隔指定的N分钟让定时器启动Service去获取服务端的最新数据。当执行到Service的onStartCommand时，在该方法内再规划一个N分钟后的定时器用于再次启动该Service并开辟一个新的线程去执行网络操作。假设Service在从服务器获取最新数据的过程中被Android系统强制杀掉，Service不会再重新创建，这也没关系，因为再过N分钟定时器就会再次启动该Service并重新获取数据

**START_STICKY** 
 如果返回START_STICKY，表示Service运行的进程被Android系统强制杀掉之后，Android系统会将该Service依然设置为started状态（即运行状态），但是不再保存onStartCommand方法传入的intent对象，然后Android系统会尝试再次重新创建该Service，并执行onStartCommand回调方法，但是onStartCommand回调方法的Intent参数为null，也就是onStartCommand方法虽然会执行但是获取不到intent信息。如果你的Service可以在任意时刻运行或结束都没什么问题，而且不需要intent信息，那么就可以在onStartCommand方法中返回START_STICKY，比如一个用来播放背景音乐功能的Service就适合返回该值。

**START_REDELIVER_INTENT** 
 如果返回START_REDELIVER_INTENT，表示Service运行的进程被Android系统强制杀掉之后，与返回START_STICKY的情况类似，Android系统会将再次重新创建该Service，并执行onStartCommand回调方法，但是不同的是，Android系统会再次将Service在被杀掉之前最后一次传入onStartCommand方法中的Intent再次保留下来并再次传入到重新创建后的Service的onStartCommand方法中，这样我们就能读取到intent参数。只要返回START_REDELIVER_INTENT，那么onStartCommand重的intent一定不是null。如果我们的Service需要依赖具体的Intent才能运行（需要从Intent中读取相关数据信息等），并且在强制销毁后有必要重新创建运行，那么这样的Service就适合返回START_REDELIVER_INTENT。

https://juejin.im/post/5b1747e5e51d45069a39ef45

 https://developer.android.com/guide/components/bound-services#kotlin 

* bindService callback

  ```java
  class BindingActivity : Activity() {
      private lateinit var mService: LocalService
      private var mBound: Boolean = false
  
      /** Defines callbacks for service binding, passed to bindService()  */
      private val connection = object : ServiceConnection {
  
          override fun onServiceConnected(className: ComponentName, service: IBinder) {
              // We've bound to LocalService, cast the IBinder and get LocalService instance
              val binder = service as LocalService.LocalBinder
              mService = binder.getService()
              mBound = true
          }
  
          override fun onServiceDisconnected(arg0: ComponentName) {
              mBound = false
          }
      }
  
      override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.main)
      }
  
      override fun onStart() {
          super.onStart()
          // Bind to LocalService
          Intent(this, LocalService::class.java).also { intent ->
              bindService(intent, connection, Context.BIND_AUTO_CREATE)
          }
      }
  
      override fun onStop() {
          super.onStop()
          unbindService(connection)
          mBound = false
      }
  
      /** Called when a button is clicked (the button in the layout file attaches to
       * this method with the android:onClick attribute)  */
      fun onButtonClick(v: View) {
          if (mBound) {
              // Call a method from the LocalService.
              // However, if this call were something that might hang, then this request should
              // occur in a separate thread to avoid slowing down the activity performance.
              val num: Int = mService.randomNumber
              Toast.makeText(this, "number: $num", Toast.LENGTH_SHORT).show()
          }
      }
  }
  ```

  get Service object in this place 

  ```java
  val binder = service as LocalService.LocalBinder
  mService = binder.getService()
  ```

  ##### Using a Messenger

   If you need your service to communicate with remote processes, then you can use a `Messenger` to provide the interface for your service. 
  
  

##### stopself()

​	目前测试只有在onStartCommand()有效，多次点击startService(intent),startId持续递增.





##### onRebind()什么情况下执行

![](SERVICE/service_binding_tree_lifecycle.png)



验证方法: startService(intent) 启动Service - bindService() 绑定Service - unBindService()解绑-  bindService() 继续绑定 就会执行onRebind()



##### 多个acitivty与service通信

https://www.jianshu.com/p/9885acf65405

https://blog.csdn.net/pihailailou/article/details/78588496



# foreground service

https://developer.android.com/develop/background-work/services/fgs

A foreground service performs some operation that is noticeable to the user. For example, an audio app would use a foreground service to play an audio track. Foreground services must display a [Notification](https://developer.android.com/develop/ui/views/notifications). Foreground services continue running even when the user isn't interacting with the app.

When you use a foreground service, you must display a notification so that users are actively aware that the service is running. This notification cannot be dismissed unless the service is either stopped or removed from the foreground.



https://developer.android.com/develop/background-work/services#Types-of-services

https://www.jianshu.com/p/5505390503fa



# background service



A background service performs an operation that isn't directly noticed by the user. For example, if an app used a service to compact its storage, that would usually be a background service.

**Note:** If your app targets API level 26 or higher, the system imposes [restrictions on running background services](https://developer.android.com/about/versions/oreo/background) when the app itself isn't in the foreground. In most situations, for example, you shouldn't [access location information from the background](https://developer.android.com/training/location/background). Instead, [schedule tasks using WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager).



codelab

https://github.com/android/codelab-android-workmanager/blob/master/app/src/main/java/com/example/background/BlurActivity.kt

https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager#12





## JobIntentService



The Android framework also provides the `IntentService` subclass of `Service` that uses a worker thread to handle all of the start requests, one at a time. Using this class is **not recommended** for new apps as it will not work well starting with Android 8 Oreo, due to the introduction of [Background execution limits](https://developer.android.com/about/versions/oreo/background#services). Moreover, it's deprecated starting with Android 11. You can use [JobIntentService](https://developer.android.com/reference/androidx/core/app/JobIntentService) as a replacement for `IntentService` that is compatible with newer versions of Android.

https://developer.android.com/develop/background-work/services#Types-of-services



In most cases, apps can work around these limitations by using [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler) jobs. This approach lets an app arrange to perform work when the app isn't actively running, but still gives the system the leeway to schedule these jobs in a way that doesn't affect the user experience

https://developer.android.com/develop/background-work/background-tasks





## WorkManger

Use WorkManager for reliable work,WorkManager handles three types of persistent work:

- **Immediate**: Tasks that must begin immediately and complete soon. May be expedited.
- **Long Running**: Tasks which might run for longer, potentially longer than 10 minutes.
- **Deferrable**: Scheduled tasks that start at a later time and can run periodically.

Figure 1 outlines how the different types of persistent work relate to one another.

https://developer.android.com/topic/libraries/architecture/workmanager



![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5srfjqyWJixvhyztBQBfyfJfxejxj6bZlR9WnEyZfLs4qmtwv1WDu1zvR-WV7aOgmjWDv7x3S9ykYhduSWem05mzqHY_fVRj3hDo0_sCi4lOD9bbHt-BVFk2I9dVL4Vue5hLL2E7UOEY/s1600/image1.png)

https://medium.com/androiddevelopers/introducing-workmanager-2083bcfc4712

https://android-developers.googleblog.com/2018/10/modern-background-execution-in-android.html

### OneTimeWork



```kotlin
// Create and enqueue the worker
val workRequest = OneTimeWorkRequestBuilder<LogWorker>().build()
workManager.enqueue(workRequest)


class LogWorker(context: Context, workerParams: WorkerParameters) : Worker(context, workerParams) {

    override fun doWork(): Result {
        Log.d("LogWorker", "Background task executed!")
        return Result.success()
    }
}
```



https://github.com/android/codelab-android-workmanager

https://developer.android.com/codelabs/basic-android-kotlin-compose-workmanager#12



## Schedule expedited work

Starting in WorkManager 2.7, your app can call `setExpedited()` to declare that a `WorkRequest` should run as quickly as possible using an expedited job. 



```kotlin
private fun scheduleExpeditedWork() {
    val workRequest: WorkRequest = OneTimeWorkRequestBuilder<LogWorker>()
        .setExpedited(OutOfQuotaPolicy.RUN_AS_NON_EXPEDITED_WORK_REQUEST)
        .build()
    WorkManager.getInstance(this).enqueue(workRequest)
}
```



### PeriodicWork



Periodic work has a minimum interval of 15 minutes.

https://developer.android.com/reference/androidx/work/PeriodicWorkRequest



```kotlin
private fun schedulePeriodicWork() {
    val periodicWorkRequest =
        PeriodicWorkRequestBuilder<LogWorker>(15, TimeUnit.MINUTES) // every 15 minutes
            .build()
    // Enqueue the work request
    WorkManager.getInstance(this).enqueue(periodicWorkRequest)
}
```

// The code is not verified by the log.



## migrate

Migrating from Firebase JobDispatcher to WorkManager 

https://developer.android.com/develop/background-work/background-tasks/persistent/migrate-from-legacy/firebase

https://medium.com/androiddevelopers/introducing-workmanager-2083bcfc4712
