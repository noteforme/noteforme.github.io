---
title: BroadcastReceiver
comments: true
date: 2017-08-12 15:44:53
tags:
categories:  ANDROID

---

#  Broadcasts
  
   
 广播使应用程序间传输传输信息的机制，主要用来监听系统或者应用发出的广播信息。
  BroadcastReceiver使用注意   当系统或应用发出广播时，扫描系统所有广播接收者(无论时静态注册还是动态注册方式)，通过action匹配將广播发给相应接收者，接收者收到
   广播后会产生一个广播实例，执行onReceiver()，这个实例生命周期只有10秒,10秒内没执行结束onReceiver，系统会报错,所以不能再onReceiver()执行耗时操作,onReceiver结束后，实例被销毁.



#  BroadcastReceiver使用方式

##  Context-registered receivers (动态注册)

* 　注册

```
    private UpdateBroadcastReceiver updateBr;
    
    @Override
    protected void onStart() {
        super.onStart();
        updateBr = new UpdateBroadcastReceiver();
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction(ACTION_UPDATE);
        registerReceiver(updateBr, intentFilter);
    }
```

* 　发送注销广播

```

    //点击发送广播
    public void btBroadcast(View v) {
        Intent intentBroad = new Intent();
        intentBroad.setAction(ACTION_UPDATE);
        sendBroadcast(intentBroad);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (updateBr != null) {
            unregisterReceiver(updateBr);
        }
    }
```
 注意:这种注册方式　AndroidManifest.xml不用做任何操作


#  Manifest-declared receivers

* AndroidManifest.xml添加

```
        <receiver android:name=".broadcastservice.UpdateBroadcastReceiver">
            <intent-filter>
                <action android:name="android.intent.action.UpdateBroadcastReceiver"/>
            </intent-filter>
        </receiver>
```
*  注意　为了杜绝APP滥用资源,官方对有的广播做了限制

>Note: If your app targets API level 26 or higher, you cannot use the manifest to declare a receiver for implicit broadcasts (broadcasts that do not target your app specifically), except for a few implicit broadcasts that are exempted from that restriction. In most cases, you can use scheduled jobs instead.

* Changes to system broadcasts
Android 7.0 and higher no longer sends the following system broadcasts. This optimization affects all apps, not only those targeting Android 7.0.

>ACTION_NEW_PICTURE
ACTION_NEW_VIDEO
Apps targeting Android 7.0 (API level 24) and higher must register the following broadcasts with registerReceiver(BroadcastReceiver, IntentFilter). Declaring a receiver in the manifest does not work.

>CONNECTIVITY_ACTION
Beginning with Android 8.0 (API level 26), the system imposes additional restrictions on manifest-declared receivers. If your app targets API level 26 or higher, you cannot use the manifest to declare a receiver for most implicit broadcasts (broadcasts that do not target your app specifically).








参考： https://developer.android.com/guide/components/broadcasts.html

[代码](http://45.77.222.97:3000/root/MineUtils/src/master/app/src/main/java/com/jonzhou/mineutils/broadcastservice/BroadcastReceiverActivity.java) 
