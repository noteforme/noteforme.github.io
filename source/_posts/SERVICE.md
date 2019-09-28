---
title: SERVICE
comments: true
date: 2017-08-11 18:21:06
tags:
categories: ANDROID

---
 Service使一个应用程序组件，再后台能执行一些耗时的操作，并且不提供用户界面，由于无法再不同的Activity对同一Thread进行控制 ，此时就要考虑服务

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

   ```
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

```
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
 08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onCreate() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30532/com.hyhy.lern I/MyIntentService: -- onStartCommand() --
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task2
08-12 13:38:26.601 30532-30655/com.hyhy.lern I/MyIntentService: do task1
08-12 13:38:26.617 30532-30532/com.hyhy.lern I/MyIntentService: -- onDestroy() --

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