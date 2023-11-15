---
title: permission_storage
date: 2023-11-15 22:52:48
tags: permission
---



https://developer.android.com/training/data-storage



android SD卡主要有两种存储方式 Internal  、  External Storage

# Internal

## 特点

* It's always available.
* Files saved here are accessible by only your app.
* When the user uninstalls your app, the system removes all your app's files from internal storage. 

内部存储不需要申请任何权限

```
        String fileInnerName = "/inner/img"; //目录名
                        //内置存储缓存目录
        File fileCache = new File(getActivity().getCacheDir(), fileInnerName); 
        //     /data/data/com.android.imageloaderstorage/cache/inner/img
         if (!fileCache.exists()) {
            boolean isInner = fileCache.mkdirs();
          }
         Log.i(" FileFragment ",  "  " + fileCache.getAbsolutePath());
```

  由此可以看到 绝对路径：

> Context.getCacheDir()：/data/data/应用包名/cache/ fileInnerName

# External Storage

 外部存储也分两种 Private files (外部私有存储)  、Public files  (外部公有存储)

  它并非始终可用，因为用户可采用 USB 存储的形式装载外部存储，并在某些情况下会从设   备中将其删除。
  它是全局可读的，因此此处保存的文件可能不受您控制地被读取。
 当用户卸载您的应用时，只有在您通过 getExternalFilesDir() 将您的应用的文件保存在 目录中时，系统才会从此处删除您的应用的文件。
 对于无需访问限制以及您希望与其他应用共享或允许用户使用电脑访问的文件，外部存储是最佳位置。 

## Private files

本属于您的应用且应在用户卸载您的应用时删除的文件。尽管这些文件在技术上可被用户和其他应用访问（因为它们在外部存储上），它们是实际上不向您的应用之外的用户提供值的文件。当用户卸载您的应用时，系统会删除应用外部专用目录中的所有文件。
例如，您的应用下载的其他资源或临时介质文件。
AndroidManifest.xml 申请权限

```
 <!-- 往SDCard写入数据权限 -->
    <uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        android:maxSdkVersion="18" />
    <!-- 在SDCard中创建与删除文件权限 -->
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />   ?需要吗?
```

https://developer.android.com/guide/topics/manifest/uses-permission-element.html

  (context.getExternalCacheDir())

```
/* Checks if external storage is available for read and write */
 String fileOutName = "/AutDir/fileOut";
                if (!isExternalStorageWritable()) {
                    return;
                }
                if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.KITKAT) {
                    File fileOut = new File(getActivity().getExternalFilesDir(Environment.DIRECTORY_PICTURES) + fileOutName);  //   /storage/emulated/0/Android/data/com.android.imageloaderstorage/files/Pictures/AutDir/fileOut
                    boolean flag = fileOut.mkdirs();
                    Log.i("FileFragment", fileOut.getAbsolutePath());
                }
                //缓存目录
                File fileOutCache = new File(getActivity().getExternalCacheDir() + fileOutName); //  /storage/emulated/0/Android/data/com.android.imageloaderstorage/cache/AutDir/fileOut
                Boolean isInner = fileOutCache.mkdirs();
                Log.i("FileFragment", " 路径 " + fileOutCache.getAbsolutePath());    
```

## Public files

  这里写入目录属于Dangerous Permissions，不能添加 `android:maxSdkVersion="18"`,否则申请权限会出问题
 `<uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>`

如果手机是6.0以下或设置 targetSdkVersion <=22,那么用下面方法可以直接创建目录

```
   /**
     * 创建外部公有目录
     */
    protected void createOutPublicDir() {
     if (!isExternalStorageWritable()) {
            return;
        }
        File filePublic = new File(Environment.getExternalStorageDirectory() + fileOutPublicName);   //   /storage/emulated/0/AutPublic/outDir
        if (!filePublic.exists()) {
            filePublic.mkdirs();
        }
    }
```

# 权限处理

      上面如果 targetSdkVersion >=23，就要考虑权限问题了

## 权限，那么就要弄清楚 compileSdkVersion 、minSdkVersion、targetSdkVersion的区别  了。

* compileSdkVersion ：
  
  需要强调的是修改 compileSdkVersion 不会改变运行时的行为。当你修改了 compileSdkVersion 的时候，可能会出现新的编译警告、编译错误，但新的 compileSdkVersion 不会被包含到 APK 中：它纯粹只是在编译的时候使用。（你真的应该修复这些警告，他们的出现一定是有原因的）

因此我们强烈推荐总是使用最新的 SDK 进行编译。在现有代码上使用新的编译检查可以获得很多好处，避免新弃用的 API ，并且为使用新的 API 做好准备。

* minSdkVersion:
  这个好理解  minSdkVersion 则是应用可以运行的最低要求。minSdkVersion 是 Google Play 商店用来判断用户设备是否可以安装某个应用的标志之一
* targetSdkVersion ：
  targetSdkVersion 是 Android 提供向前兼容的主要依据,就像后面在“外置公有目录”创建快捷方式，targetSdkVersion 22可以创建，targetSdkversion 23不能创建

## 权限设置代码

  外置公有目录（Environment.getExternalStorageDirectory()）

```
     // Here, thisActivity is the current activity
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                    if (ContextCompat.checkSelfPermission(getActivity(), Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
                        // Should we show an explanation?
                        if (ActivityCompat.shouldShowRequestPermissionRationale(getActivity(), Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
//                            当某条权限之前已经请求过，并且用户已经拒绝了该权限时，shouldShowRequestPermissionRationale ()方法返回的是true
                        } else {
//                            ActivityCompat.requestPermissions(getActivity(), new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, WRITE_EXTERNAL_STORAGE_REQUEST_CODE);
                            requestPermissions(new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, WRITE_EXTERNAL_STORAGE_REQUEST_CODE);
                        }
                    }
                } else {
                    createOutPublicDir();
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
                    createOutPublicDir();
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

如果我们在低于23的手机上运行，那么它安装试默认给定权限，targetSdkVersion 不管怎么改都没用了

 A、权限这块每次都要卸载 然后运行

B、我们通常在通常在Frament 调用 requestPermissions发起权限，但是在onRequestPermissionsResult没有回调     所以要用 Frament的 requestPermissions方法

3、getExternalStorageDirectory和getExternalStoragePublicDirectory的区别

>   String vUrl = Environment.getExternalStorageDirectory().getPath() + 
> "/mv.mp4";    String vm =
> Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_MOVIES).getPath()+
> "/mv.mp4";

   打印出来的路径：
    /storage/emulated/0/mv.mp4
     /storage/emulated/0/Movies/mv.mp4
多了个Movies目录，其实使用不同的Type就有不同的目录，像 Alarms、DCIM、Download、Movies、Music、Notifications、Pictures、Podcasts、Ringtones九大目录

## android:maxSdkVersion="18"原因

处理的是  `WRITE_EXTERNAL_STORAGE   getExternalFilesDir(String) and getExternalCacheDir()`

* 如果要申请  内置目录读写在api>=19是不需要声明权限的，但是为了兼容
  
        为了兼容android版本 ，通常minSdkVersion 14，那么就要声明`WRITE_EXTERNAL_STORAGE`  权限了
        然而声明之后我们在设置里是不是会看到这（借的图）
        [外链图片转存失败(img-Lyvi1FAk-1568797653293)(http://unclechen.github.io/content/images/app-perm-before.png)]
        如果持有6.0手机的用户把 存储空间 给关了，而我刚好没适配6.0就以 targetSdkVersion 22打包，那么应用就没办法使用内置存储空间了，应用基本跑不了
* 加上`android:maxSdkVersion="18"`后
  
            ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL3VuY2xlY2hlbi5naXRodWIuaW8vY29udGVudC9pbWFnZXMvYXBwLXBlcm0tYWZ0ZXIucG5n?x-oss-process=image/format,png)
       就没这个按钮关了
* 但是有个问题，如果要使用外置存储目录（Environment.getExternalStorageDirectory()）　android:maxSdkVersion="18"就不能加了，否则没法使用了，授权框也不会弹出来
  参考:https://developer.android.com/reference/android/Manifest.permission.html#WRITE_EXTERNAL_STORAGE

**特别注意** mkdir(),mkdirs(),createNewFile()的区别

*  createNewFile:新建文件（非目录）

*  mkdir：新建目录

* mkdirs：新建目录，与mkdir的区别是：比如   mkdirs（“D:/test/test2”) 如果test  ,   不存在会创建，然后创建test2，如果是  mkdir（“D:/test/test2”) ，如果 
  
               test不存在，会失败。

* android io输入文件
  
  ```
  new File(param1,param2); param1 File,param2  String类型
  
  String dir = StorageUtils.getCacheDirectory(this);
  String apkDIR = "apkdir.apk";
  File apkFile = new File(dir, apkDIR);
  ```

参考：

https://developer.android.com/training/permissions/requesting.html
https://developer.android.com/guide/topics/permissions/requesting.html
https://developer.android.com/training/basics/data-storage/files.html?hl=zh-cn

http://www.jianshu.com/p/2de0113b3164
http://unclechen.github.io/2016/03/05/Android6.0%E8%BF%90%E8%A1%8C%E6%97%B6%E6%9D%83%E9%99%90%E7%AE%80%E4%BB%8B/
http://chinagdg.org/2016/01/picking-your-compilesdkversion-minsdkversion-targetsdkversion/
