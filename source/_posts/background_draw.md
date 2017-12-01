---
title: background_draw (背景图形绘制)
comments: true
date: 2017-11-03 09:59:57
tags:
categories:  ANDROID
---

为了App性能，经常会用到代码写背景

代码里设置颜色  `Color.parseColor("#fa6d62")`


#　设置Button按钮背景

![登录背景](background_draw/bg_login.png)

```
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <corners android:radius="20dp" />

    <!-- 边框的颜色-->
    <stroke
        android:width="2dp"
        android:color="#009cff" />
    <!--填充的颜色 -->
    <solid android:color="@android:color/transparent" />
</shape>
```

#  layer-list  

LayerDrawable 是管理其他可绘制对象阵列的可绘制对象。列表中的每个可绘制对象按照列表的顺序绘制，列表中的最后一个可绘制对象绘于顶部。每个可绘制对象由单一 元素内的 元素表示。我们需要注意的是layer-list中有item的先后顺序会影响展示效果，不同顺序的效果可能大相径庭，因为，后面的item总是在之前的item之上并覆盖显示。

http://blog.csdn.net/xiehuimx/article/details/70242676


# 阴影效果

前端时间一直找这种资料，效果一直不如意，不得已用了图片，真是　踏破铁鞋无觅处
https://developer.android.com/training/material/shadows-clipping.html


# 画线

https://lianyuchen.github.io/2017/05/09/%E5%85%B3%E4%BA%8Eshape%E7%94%BB%E8%99%9A%E7%BA%BF/