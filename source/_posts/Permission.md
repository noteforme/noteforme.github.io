---
title: Permission
comments: true
date: 2017-12-02 20:11:32
tags:
categories: ANDROID
---


#  权限管理 

  6.0后最大的改变

## 危险权限:



##  文件存储文章
http://blog.csdn.net/huaiyiheyuan/article/details/52473984
https://developer.android.com/training/permissions/requesting.html

##   基本使用
```

        // Here, thisActivity is the current activity
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
        		//则每次执行需要这一权限的操作时您都必须检查自己是否具有该权限
            if (ContextCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                // Should we show an explanation?
                if (ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
//                            当某条权限之前已经请求过，并且用户已经拒绝了该权限时，shouldShowRequestPermissionRationale ()方法返回的是true
                } else {
//                            ActivityCompat.requestPermissions(getActivity(), new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, WRITE_EXTERNAL_STORAGE_REQUEST_CODE);
                    requestPermissions(new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, WRITE_EXTERNAL_STORAGE_REQUEST_CODE);
                }
            } else {
                handlerExecute();
            }
        }
    

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        switch (requestCode) {
            case WRITE_EXTERNAL_STORAGE_REQUEST_CODE: {
                // If request is cancelled, the result arrays are empty.
                if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    // permission was granted, yay! Do the
                    // contacts-related task you need to do.
                    handlerExecute();
                } else {
                    // permission denied, boo! Disable the
                    // functionality that depends on this permission.
                }
                return;
                // other 'case' lines to check for other
                // permissions this app might request
            }
        }
    }

```







## 权限

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
--------------------- 
作者：lasebella 
来源：CSDN 
原文：https://blog.csdn.net/lasebella/article/details/73867331 
版权声明：本文为博主原创文章，转载请附上博文链接！
```



####  多权限申请

https://github.com/permissions-dispatcher/PermissionsDispatcher

https://github.com/googlesamples/android-RuntimePermissions

https://www.jianshu.com/p/a51593817825





https://developer.android.com/training/permissions/requesting?hl=zh-cn



livedata

https://mp.weixin.qq.com/s/Hw_FI0GRpUDWJ1NwjK652w

https://mp.weixin.qq.com/s/7RpGzTjXo9rnHRCVEnrYWQ