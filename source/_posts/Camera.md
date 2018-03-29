---
title: Camera
comments: true
date: 2018-03-28 22:16:43
tags:
categories: ANDROID
---
### 官方资料

https://developer.android.com/training/camera/photobasics.html#TaskPath



### 相册选择

```
 public static final int REQUEST_IMAGE_CAPTURE = 1;

 private void btAlbum() {
        Intent takePictureIntent = new Intent(
                Intent.ACTION_PICK,
                MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
        startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
    }
```

### 

```
  @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            if (requestCode == REQUEST_IMAGE_CAPTURE && null != data) {
                Uri selectedImage = data.getData();
                String[] filePathColumn = {MediaStore.Images.Media.DATA};
                Cursor cursor = this.getContentResolver().query(selectedImage, filePathColumn,
                        null, null, null, null);
                cursor.moveToFirst();
                int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
                final String picturePath = cursor.getString(columnIndex);
                cursor.close();
                Bitmap bitmap = BitmapFactory.decodeFile(picturePath);
                ivImg.setImageBitmap(bitmap);
            }
        }
    }
```

### 拍照选择

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







参考:https://www.jianshu.com/p/f269bcda335f#

https://www.jianshu.com/p/26d29e187f89

servlet  
https://my.oschina.net/u/2008084/blog/524937

7.0 
https://blog.csdn.net/u011418943/article/details/77712662

