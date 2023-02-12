---
title: NewFeatures
comments: true
date: 2017-12-17 22:26:11
tags:  AndroidNewFeatures
categories:  ANDROID
---

 ANDROID新特性

### apk安装

  	 StrictMode API政策


* 在AndroidManifest.xml添加

```
   <provider
            android:name="android.support.v4.content.FileProvider"   
            android:authorities="${applicationId}.fileProvider"         // com.cqian是包名
            android:grantUriPermissions="true"
            android:exported="false">
            <meta-data android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_install"/>
        </provider>
```

* 编写provider_install.xml文件　
   新建xml指定共享目录

   有下面几种存储方式, name不变，path就是下面得apkCacheFile

```
<files-path name="name" path="path" /> 物理路径相当于Context.getFilesDir() + /path/  
  
<cache-path name="name" path="path" /> 物理路径相当于Context.getCacheDir() + /path/  
  
<external-path name="name" path="path" /> 物理路径相当于Environment.getExternalStorageDirectory() + /path/  
  
<external-files-path name="name" path="path" /> 物理路径相当于Context.getExternalFilesDir(String) + /path/  
  
<external-cache-path name="name" path="path" /> 物理路径相当于Context.getExternalCacheDir() + /path/  

```

 由于我的存储目录用这个，apkCacheFile是SDk卡上得目录名称

 `String downFile = context.getExternalCacheDir() + "apkCacheFile"` 



```
   <external-cache-path
        name="name"
        path="apkCacheFile" />
```

* 安装APK

  就这个安装方法

```
  private static void installApk(Context context, File apkFile) {
        Intent intent = new Intent();
        // 由于没有在Activity环境下启动Activity,设置下面的标签
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        intent.setAction(Intent.ACTION_VIEW);
        String type = "application/vnd.android.package-archive";

        Uri uri;
        if (Build.VERSION.SDK_INT >= 24) {
            // 参数2 清单文件中provider节点里面的authorities ; 参数3  共享的文件,即apk包的file类
            uri = FileProvider.getUriForFile(context, "getPackageName().fileProvider", apkFile);
            //对目标应用临时授权该Uri所代表的文件
            intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        } else {
            uri = Uri.fromFile(apkFile);
        }
        intent.setDataAndType(uri, type);
        context.startActivity(intent);
    }
```




https://developer.android.google.cn/about/versions/nougat/android-7.0-changes.html

http://blog.csdn.net/github_2011/article/details/74297460

#####  图片拍照

Android P veridex工具使用

win10安装ubuntu

<https://www.linuxidc.com/Linux/2018-03/151256.htm>

#####  对test.apk进行扫描

> ./appcompat.sh –dex-file=test.apk

* 在cmd里敲bash，找到/mnt，你会发现下面有c d e f ，这些就是你win下的硬盘，你可以直接对win下的文件进行操作，包括执行一些linux指令或者编译Linux程序

* 用mv /mnt/x/a/b-c.zip /home/user/b.zip这类命令

  示例:

  <http://www.10tiao.com/html/227/201812/2650244825/1.html>

###  android 11适配

https://juejin.im/post/6860370635664261128





### android12适配

##### Splash

https://www.youtube.com/watch?v=8F0lK2i3VJk

https://developer.android.com/develop/ui/views/launch/splash-screen#elements