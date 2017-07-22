---
title: glide加载图片
date: 2017-07-21 14:03:20
tags:
categories: "ANDROID"

---
https://github.com/bumptech/glide

 通常加载图片写个类简单封装下，为了更换库时减少修改

    public class ImageLoader {

     public static void showImage(Context context, String imgUrl, ImageView imageView) {
        Glide.with(context).load(imageUrl).into(imageView);
     }
    }