---
title: SERVICE
comments: true
date: 2017-08-11 18:21:06
tags:
categories: ANDROID

---
 Service使一个应用程序组件，再后台能执行一些耗时的操作，并且不提供用户界面，由于无法再不同的Activity对同一Thread进行控制 ，此时就要考虑服务

#### Service启动服务两种方式

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

https://developer.android.com/training/run-background-service/create-service.html