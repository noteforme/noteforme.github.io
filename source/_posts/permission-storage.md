---
title: permission_storage
date: 2023-11-15 22:52:48
tags: permission
---

The answer is NO it is not required if you are simply capturing image from camera or by gallery by calling an intent (calling other app to perform some task for you ) in our case this other app would be the camera application in your mobile

https://medium.com/@bilalhameed0800/is-permission-required-for-android-capture-image-from-camera-and-gallery-intent-4812964e8c9a

https://developer.android.com/training/data-storage

[Preparing for scoped storage (Android Dev Summit &#39;19) - YouTube](https://www.youtube.com/watch?v=UnJ3amzJM94)

android SD卡主要有两种存储方式 Internal  、  External Storage

external storage permission

自己创建的文件，不需要权限 ，不是自己的 需要 READ_EXTERNAL_STORAGE

#### photo picker

https://developer.android.com/about/versions/14/changes/partial-photo-video-access

if your app is running on a device with Android 14 (API level 34) or higher, limit access to selected photos and videos, request "Manifest.permission.READ_MEDIA_IMAGES" will show 

<img src="permission-storage/b699ef581b9e92de2ecf20ac6ca6c5278c439c98.png" title="" alt="Screenshot 2023-11-22 at 21.36.55.png" width="259">

##### 

#### Internal

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

[Android 13媒体文件访问权限适配_read_media_images-CSDN博客](https://blog.csdn.net/github_27263697/article/details/131633314)

[Android 13 媒体权限适配指南 - 掘金](https://juejin.cn/post/7159999910748618766)

### READ_MEDIA_VISUAL_USER_SELECTED

这个限制和 iOS 相似，Android 14 提供了对照片和视频的部分访问权限。当您访问媒体数据时，用户将看到一个对话框，提示用户授予对所有媒体的访问、或者授予单个照片/视频的访问权限，该新功能将适用于 Android 14 上所有应用程序，无论其 targetSdkVersion 是多少

![The .](https://developer.android.google.cn/static/about/versions/14/images/partial-photo-video-access.png)

    谷歌在 API 33（Android 13）上面引入了 READ_MEDIA_IMAGES 和 READ_MEDIA_VIDEO 这两个权限，目前针对这两个权限在 Android 14 上面有新的变动，具体的变动点就是新增了 READ_MEDIA_VISUAL_USER_SELECTED权限，那么这个权限的作用是什么呢？

我们都知道 READ_MEDIA_IMAGES 和 READ_MEDIA_VIDEO 是申请图片和视频权限的，但是这样会有一个问题，当第三方应用申请到权限后，就拥有了手机相册中所有照片和视频的访问权限，这是十分危险的，也是非常不可控的，因为用户也无法知道第三方应用会干什么，所以谷歌在 API 34（Android 14）引入了这个权限，这样用户拥有了更多的选择，可以将相册中所有的图片和视频授予给第三方应用，也可以将部分的图片和视频给第三方应用。

讲完了这个特性的来龙去脉，那么接下来讲讲这个权限如何适配，如果你的应用申请了 READ_MEDIA_IMAGES 或者 READ_MEDIA_VIDEO 权限，并且 targetSdkVersion 大于等于 33（Android 13），那么需要在申请权限时携带上 READ_MEDIA_VISUAL_USER_SELECTED 权限方能正常申请，如果不携带上 READ_MEDIA_VISUAL_USER_SELECTED 权限就申请 READ_MEDIA_IMAGES 或者 READ_MEDIA_VIDEO 权限，会弹出权限询问对话框，但是如果用户是选择全部授予，那么 READ_MEDIA_IMAGES 或者 READ_MEDIA_VIDEO 权限状态是已授予的状态，如果用户是选择部分授予，那么 READ_MEDIA_IMAGES 或者 READ_MEDIA_VIDEO 权限状态是已拒绝的状态，假设此时有携带了 READ_MEDIA_VISUAL_USER_SELECTED 权限的情况下，那么 READ_MEDIA_VISUAL_USER_SELECTED 权限是已授予的状态。

看到这里，脑洞大的同学可能有想法了，那我不申请 READ_MEDIA_IMAGES 或者 READ_MEDIA_VIDEO 权限，我就只申请 READ_MEDIA_VISUAL_USER_SELECTED 权限行不行啊？答案也是不行的，我替大家试验过了，这个权限申请会在不会询问用户的情况下，被系统直接拒绝掉。

另外需要的一点是 READ_MEDIA_VISUAL_USER_SELECTED 属于危险权限，除了在运行时动态申请外，还需要在清单文件中进行注册。

[[Bug]：安卓14，申请READ_MEDIA_IMAGES时，当用户选择部分允许后，READ_MEDIA_IMAGES的isGranted=true · Issue #244 · getActivity/XXPermissions · GitHub](https://github.com/getActivity/XXPermissions/issues/244)

```
   var filter by remember { mutableStateOf<VisualMediaType>(ImageAndVideo) }


   /**
     * [PickMultipleVisualMedia] is an ActivityResultContract that will launch the photo picker
     * intent while providing an enhanced API over the available customisations options (media type,
     * max number of items).
     *
     * If you're looking for single selection only, use PickVisualMedia ActivityResultContract
     * instead.
     */
    val pickMultipleMedia =
        rememberLauncherForActivityResult(PickMultipleVisualMedia(maxItems)) { uris ->
            selectedMedia = uris
        }



    pickMultipleMedia.launch(PickVisualMediaRequest(filter))   
```
