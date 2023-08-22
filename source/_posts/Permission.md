---
title: Permission
comments: true
date: 2017-12-02 20:11:32
tags:
categories: ANDROID
---

https://developer.android.com/training/permissions/requesting

https://developer.android.com/guide/topics/permissions/overview#normal-dangerous



#### 权限管理 

##### Normal permissions



##### Dangerous permissions

<img src="Permission/Screen Shot 2020-09-17 at 2.06.37 PM.png" alt="Screen Shot 2020-09-17 at 2.06.37 PM" style="zoom: 80%;" />

#### Android11权限申请

申请位置权限，先申请前台，后申请后台权限。

https://zhuanlan.zhihu.com/p/275758740

####   基本使用



```java
 public static final int PERMISSION_REQUEST_CODE = 1000;
    void requestPermission() {
        // Here, thisActivity is the current activity
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            //则每次执行需要这一权限的操作时您都必须检查自己是否具有该权限
            if (ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                // Should we show an explanation?
                if (ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
//                            当某条权限之前已经请求过，并且用户已经拒绝了该权限时
                    //异步向用户显示解释，这里先弹出解释框，再请求权限.
                    Log.d(TAG, "用户看到解释后，再次常识请求权限");
                    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, PERMISSION_REQUEST_CODE);
                } else {
                    //无需解释，首次请求许可
                    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, PERMISSION_REQUEST_CODE);
                    Log.d(TAG, "requestPermissions");
                }
            } else {
                //权限已经授予
                Log.d(TAG, "requestPermissions is ok");
            }
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        switch (requestCode) {
            case PERMISSION_REQUEST_CODE: {
                // If request is cancelled, the result arrays are empty. 如果取消请求，则grantResult 为空
                if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    Log.d(TAG, "onRequestPermissionResult:ok");
                } else {
                    Log.d(TAG, "onRequestPermissionResult:no");
                }
                return;
            }
        }
    }
```





> 注意:需要在androidmanifest.xml声明 否则一直返回false

```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

1、检查权限是否授予。

```
Activity.java
public int checkSelfPermission(permission)
```

2、申请权限。

```
Activity.java    
public final void requestPermissions( new String[permission1,permission2,...], requestCode)
```

这个时候，会弹出系统授权弹窗（**授权弹窗是不支持自定义的，原因理所当然**）。

3、权限回调。

用户在系统弹窗里面选择后，结果会通过`Activity`的 `onRequestPermissionsResult`方法回调APP。

```
public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    //继续执行逻辑或者提示权限获取失败
}
```

4、权限说明。

用户如果选择了拒绝，下一次在需要声明该权限的时候，Google建议APP开发者给予用户更多的说明，因此提供了下面这个API,**这个方法返回值在使用过程中会发现有点纠结（具体解析见下面代码块说明）**。

```
public boolean shouldShowRequestPermissionRationale(permission)
{
    1、APP没有申请这个权限的话，返回false
    2、用户拒绝时，勾选了不再提示的话，返回false
    3、用户拒绝，但是没有勾选不再提示的话，返回true
    因此如果想在第一次就给用户提示，需要记录权限是否申请过，没有申请过的话，强制弹窗提示，而不能根据这个方法的返回值来。
}
```

这个 看情况选用

参考 https://juejin.im/entry/58b2e490ac502e0069d9ae62

https://developer.android.com/guide/topics/security/permissions?hl=zh-cn#normal-dangerous

http://www.10tiao.com/html/227/201610/2650237473/1.html

####  PERMISSION

​      

```
  <!-- 这个权限用于进行网络定位-->
    <permission android:name="android.permission.ACCESS_COARSE_LOCATION"></permission>
    <!-- 这个权限用于访问GPS定位-->
    <permission android:name="android.permission.ACCESS_FINE_LOCATION"></permission>
    <!-- 用于访问wifi网络信息，wifi信息会用于进行网络定位-->
    <permission android:name="android.permission.ACCESS_WIFI_STATE"></permission>
    <!-- 获取运营商信息，用于支持提供运营商信息相关的接口-->
    <permission android:name="android.permission.ACCESS_NETWORK_STATE"></permission>
    <!-- 这个权限用于获取wifi的获取权限，wifi信息会用来进行网络定位-->
    <permission android:name="android.permission.CHANGE_WIFI_STATE"></permission>
    <!-- 用于读取手机当前的状态-->
    <permission android:name="android.permission.READ_PHONE_STATE"></permission>
    <!-- 写入扩展存储，向扩展卡写入数据，用于写入离线定位数据-->
    <permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></permission>
    <!-- 访问网络，网络定位需要上网-->
    <permission android:name="android.permission.INTERNET" />
    <!-- SD卡读取权限，用户写入离线定位数据-->
    <permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"></permission>

```



##### One-time permissions

Starting in Android 11 (API level 30), whenever your app requests a permission related to location, microphone, or camera, the user-facing permissions dialog contains an option called **Only this time**. If the user selects this option in the dialog, your app is granted a temporary *one-time permission*.



##### Permission groups

- If the app has already been granted another dangerous permission in the same permission group, the system immediately grants the permission without any interaction with the user. For example, if an app had previously requested and been granted the `READ_CONTACTS` permission, and it then requests `WRITE_CONTACTS`, the system immediately grants that permission without showing the permissions dialog to the user.

- **Caution:** Future versions of the Android SDK might move a particular permission from one group to another. Therefore, don't base your app's logic on the structure of these permission groups.

  For example, `READ_CONTACTS` is in the same permission group as `WRITE_CONTACTS` as of Android 8.1 (API level 27). If your app requests the `READ_CONTACTS` permission, and then requests the `WRITE_CONTACTS` permission, don't assume that the system can automatically grant the `WRITE_CONTACTS` permission.



####  多权限申请

https://github.com/permissions-dispatcher/PermissionsDispatcher

https://github.com/googlesamples/android-RuntimePermissions

https://www.jianshu.com/p/a51593817825

https://developer.android.com/training/permissions/requesting



livedata

https://mp.weixin.qq.com/s/Hw_FI0GRpUDWJ1NwjK652w

https://mp.weixin.qq.com/s/7RpGzTjXo9rnHRCVEnrYWQ

https://mp.weixin.qq.com/s/i8K-6CSxYuLanR4x1WNSGA

https://mp.weixin.qq.com/s/gaSmpT5UQLqNa4Ck0r0eOg



http://blog.csdn.net/huaiyiheyuan/article/details/52473984
https://developer.android.com/training/permissions/requesting.html



#### Android 10权限

在后台运行时访问设备位置信息需要权限

` <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>`		



该权限允许应用程序在后台访问位置。如果请求此权限，则还必须请求`ACCESS_FINE_LOCATION` 或 `ACCESS_COARSE_LOCATION`权限。只请求此权限无效果

在Android 10的设备上，如果你的应用的 `targetSdkVersion` < 29，则在请求`ACCESS_FINE_LOCATION` 或`ACCESS_COARSE_LOCATION`权限时，系统会自动同时请求`ACCESS_BACKGROUND_LOCATION`。在请求弹框中，选择“始终允许”表示同意后台获取位置信息，选择“仅在应用使用过程中允许”或"拒绝"选项表示拒绝授权。



如果你的应用的 `targetSdkVersion` >= 29，则请求`ACCESS_FINE_LOCATION` 或 `ACCESS_COARSE_LOCATION`权限表示在前台时拥有访问设备位置信息的权限。在请求弹框中，选择“始终允许”表示前后台都可以获取位置信息，选择“仅在应用使用过程中允许”只表示拥有前台的权限。



https://developer.android.com/about/versions/10/privacy/changes

https://juejin.im/post/6844904073024503822#heading-5


request permission demo
https://github.com/android/platform-samples/tree/main/samples/privacy/permissions



### Custom Permission

can run Demo

https://github.com/Ibtisam/Dangerous-Activity

https://github.com/Ibtisam/Using-Dangerous-Activity





https://github.com/commonsguy/cwac-netsecurity

https://medium.com/code-procedure-and-rants/android-permissions-create-custom-permissions-5784188311f3



https://www.youtube.com/watch?v=YeLmufMZfcI

https://stackoverflow.com/questions/28975706/start-activity-with-permissions

https://stackoverflow.com/questions/26700767/custom-permission-for-activity

