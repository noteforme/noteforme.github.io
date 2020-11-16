---
title: Glide
date: 2017-07-21 14:03:20
tags:
categories: ANDROID

---
https://github.com/bumptech/glide

 通常加载图片写个类简单封装下，为了更换库时减少修改

    public class ImageLoader {
    
     public static void showImage(Context context, String imgUrl, ImageView imageView) {
        Glide.with(context).load(imageUrl).into(imageView);
     }
    }


## 加载占位符

https://muyangmin.github.io/glide-docs-cn/doc/generatedapi.html

```
implementation 'com.github.bumptech.glide:glide:4.6.1'
annotationProcessor 'com.github.bumptech.glide:compiler:4.6.1'
```


https://juejin.im/post/6844904136266219534



https://www.jianshu.com/p/48d9e4d5d75d