---
title: BroadcastReceiver
comments: true
date: 2017-08-12 15:44:53
tags:
categories:  ANDROID

---

BroadcastReceiver

 https://developer.android.com/guide/components/broadcasts.html



 广播使应用程序间传输传输信息的机制，主要用来监听系统或者应用发出的广播信息。
  BroadcastReceiver使用注意   当系统或应用发出广播时，扫描系统所有广播接收者(无论时静态注册还是动态注册方式)，通过action匹配將广播发给相应接收者，接收者收到
   广播后会产生一个广播实例，执行onReceiver()，这个实例生命周期只有10秒,10秒内没执行结束onReceiver，系统会报错,所以不能再onReceiver()执行耗时操作,onReceiver结束后，实例被销毁.



####  BroadcastReceiver注册方式

#####  Context-registered receivers (动态注册)

注册

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

发送注销广播

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


#####  静态注册

Android 8.0之后，静态注册是无法接收隐式广播的，而默认情况下，我们发出的广播都是隐式广播，因此一定要调用setPackage()方法，指定这条广播是发送给哪个应用程序的，从而让它变成一条显示广播。（第一行代码）

AndroidManifest.xml添加

```
        <receiver android:name=".broadcastservice.UpdateBroadcastReceiver">
            <intent-filter>
                <action android:name="android.intent.action.UpdateBroadcastReceiver"/>
            </intent-filter>
        </receiver>
```
注意　为了杜绝APP滥用资源,官方对有的广播做了限制

>Note: If your app targets API level 26 or higher, you cannot use the manifest to declare a receiver for implicit broadcasts (broadcasts that do not target your app specifically), except for a few implicit broadcasts that are exempted from that restriction. In most cases, you can use scheduled jobs instead.

```kotlin
intent.setPackage("com.comm.util")
```



#### Sending broadcasts方式

##### 普通广播

##### 有序广播

FirstOrderBroadCast

```kotlin
class FirstOrderBroadCast : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Log.e("receive", "Low");
        val code = 1
        val data = "hello I am FirstOrderBroadCast"
        val bundle: Bundle? = Bundle()
        bundle?.putString("first","from FirstOrderBroadCast")
        setResult(code, data, bundle)
    }
}
```

SecondOrderBroadCast

```kotlin
class SecondOrderBroadCast : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Log.e("receive", "Mid")
        //获取上一个广播接收器结果
        val code = resultCode
        val data = resultData
        var bundleResult = getResultExtras(true).get("first");

        Log.e("receive", "获取到上一个广播接收器结果：code=  $code   data= $data  bundleResult $bundleResult")
    }
}
```

ThirdOrderBroadCast

```kotlin
class ThirdOrderBroadCast : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Log.e("receive", "High")
        //传递结果到下一个广播接收器
        val code = 3
        val data = "hello"
        val bundle: Bundle? = null
        setResult(code, data, bundle)
    }
}
```

AndroidMnifest.xml

```xml
<receiver android:name=".component.broadcastservice.FirstOrderBroadCast">
    <intent-filter android:priority="3000">
        <action android:name="com.broadcast.android" />
    </intent-filter>
</receiver>
<receiver android:name=".component.broadcastservice.SecondOrderBroadCast">
    <intent-filter android:priority="2000">
        <action android:name="com.broadcast.android" />
    </intent-filter>
</receiver>
<receiver android:name=".component.broadcastservice.ThirdOrderBroadCast">
    <intent-filter android:priority="1000">
        <action android:name="com.broadcast.android" />
    </intent-filter>
</receiver>
```

or

```kotlin
updateBr = UpdateBroadcastReceiver()
val intentFilter = IntentFilter()
intentFilter.addAction(UpdateBroadcastReceiver.ACTION_UPDATE)
registerReceiver(updateBr, intentFilter)
```

使用

BroadcastReceiverActivity

```kotlin
val intent = Intent()
intent.action = "com.broadcast.android"

bt_order_broadcast.setOnClickListener {
    sendOrderedBroadcast(intent,null)
}
```

运行结果

```
 E/receive: Low
 E/receive: Mid
 E/receive: 获取到上一个广播接收器结果：code=  1   data= hello I am FirstOrderBroadCast  bundleResult from FirstOrderBroadCast
 E/receive: High
```

##### [LocalBroadcastManager.sendBroadcast](https://developer.android.com/reference/androidx/localbroadcastmanager/content/LocalBroadcastManager#sendBroadcast(android.content.Intent))

method sends broadcasts to receivers that are in the same app as the sender. If you don't need to send broadcasts across apps, use local broadcasts.he implementation is much more efficient (no interprocess communication needed) and you don't need to worry about any security issues related to other apps being able to receive or send your broadcasts.(应用内发送广播)

发送

```kotlin
val intentBroad = Intent()
intentBroad.action = UpdateBroadcastReceiver.ACTION_UPDATE
LocalBroadcastManager.getInstance(this).sendBroadcast(intentBroad)
```

注册 

```kotlin
updateBr = UpdateBroadcastReceiver()
val intentFilter = IntentFilter()
intentFilter.addAction(UpdateBroadcastReceiver.ACTION_UPDATE)
registerReceiver(updateBr, intentFilter)
updateBr?.let {
    LocalBroadcastManager.getInstance(this).registerReceiver(it,intentFilter)
}
```

https://blog.csdn.net/qq_30379689/article/details/53341313