---
title: permission_storage
date: 2023-11-15 22:52:48
tags: permission
---

https://developer.android.com/training/data-storage

[Preparing for scoped storage (Android Dev Summit &#39;19) - YouTube](https://www.youtube.com/watch?v=UnJ3amzJM94)

android SD卡主要有两种存储方式 Internal  、  External Storage

external storage permission

自己创建的文件，不需要权限 ，不是自己的 需要 READ_EXTERNAL_STORAGE

##### 

##### Internal

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

### External Storage

 外部存储也分两种 Private files (外部私有存储)  、Public files  (外部公有存储)

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

**特别注意** mkdir(),mkdirs(),createNewFile()的区别

* createNewFile:新建文件（非目录）

* mkdir：新建目录

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
