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
https://developer.android.com/guide/topics/permissions/requesting.html#normal-dangerous


##  文件存储文章
http://blog.csdn.net/huaiyiheyuan/article/details/52473984
https://developer.android.com/training/permissions/requesting.html

##   基本使用
```

        // Here, thisActivity is the current activity
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
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