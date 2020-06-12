---
title: Camera
comments: true
date: 2017-08-28 22:16:43
tags:
categories: ANDROID
---
https://developer.android.com/training/camera/photobasics.html#TaskPath

#####  Get the thumbnail(缩略图)

```
 public static final int REQUEST_IMAGE_CAPTURE = 1;

 private void btAlbum() {
        Intent takePictureIntent = new Intent(
                Intent.ACTION_PICK,
                MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
        startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
 }
```



```
  @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
     if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == Activity.RESULT_OK) {
              data?.extras?.let {
                  val imageBitmap = it.get("data") as Bitmap
                  img_show.setImageBitmap(imageBitmap)
         }
    }
```

#####  Save the full-size photo

  	provider_camera.xml 

```
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name="my_images"
        path="Android/data/com.zhujia.land/files/Pictures" />
</paths>
```

跳转拍照 : 还得注意申请权限

```
 private void dispatchTakePictureIntent(int num) {
        Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
        if (takePictureIntent.resolveActivity(this.getPackageManager()) != null) {
            try {
                photoFile = createImageFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
            if (photoFile == null) {
                return;
            }
            Uri imgUri;
            if (Build.VERSION.SDK_INT < 24) {
                imgUri = Uri.fromFile(photoFile);
            } else {
                //Android 7.0系统开始 使用本地真实的Uri路径不安全,使用FileProvider封装共享Uri
                //参数二:fileprovider绝对路径 com.dyb.testcamerademo：项目包名
                imgUri = FileProvider.getUriForFile(this, "com.zhujia.land.fileprovider", photoFile);
            }
            takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, imgUri);
        }
        startActivityForResult(takePictureIntent, num);
    }
```

```
  override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == Activity.RESULT_OK) {
            //如果拍照成功，将Uri用BitmapFactory的decodeStream方法转为Bitmap
            val imageBitmap = BitmapFactory.decodeStream(photoURI?.let {
                contentResolver.openInputStream(
                    it
                )
            })
             galleryAddPic(mImageUriFromFile) //将拍的照片添加到相册
            img_show.setImageBitmap(imageBitmap)
        }
    }
```



照片添加到相册

```
/**
 * 将拍的照片添加到相册
 *
 * @param uri 拍的照片的Uri
 */
private fun galleryAddPic(uri: Uri?) {
    val mediaScanIntent = Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE,uri)
    sendBroadcast(mediaScanIntent)
}
```

But it has a question

```
  fun createImageFile(context: Context): File? {
            // Create an image file name
            val timeStamp: String = SimpleDateFormat(yyyyMMDD_HHmmss).format(Date())
//            val storageDir: File? = context.getExternalFilesDir(Environment.DIRECTORY_PICTURES)
            val storageDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES)

            return File.createTempFile(
                "JPEG_${timeStamp}_", /* prefix */
                ".jpg", /* suffix */
                storageDir /* directory */
            ).apply {
                // Save a file: path for use with ACTION_VIEW intents
                var currentPhotoPath = absolutePath
                Timber.i("createImageFile  $currentPhotoPath")
            }
        }
```

`ACTION_MEDIA_SCANNER_SCAN_FILE` 还有一个限制条件，那就是传递进去的文件绝对路径，必须是以 `Environment.getExternalStorageDirectory()` 方法的返回值开头。

这样 'context.getExternalFilesDir(Environment.DIRECTORY_PICTURES)'  这个暂时没弄好

https://juejin.im/post/5ae0541df265da0b9d77e45a



https://www.jianshu.com/p/f269bcda335f#

https://www.jianshu.com/p/26d29e187f89

servlet  
https://my.oschina.net/u/2008084/blog/524937

7.0 
https://blog.csdn.net/u011418943/article/details/77712662

Android 10

https://mp.weixin.qq.com/s/31esIqMudRRDBY8JDs8D4A



https://juejin.im/post/5a33a5106fb9a04525782db5