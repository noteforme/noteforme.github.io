---
title: Popupwindow
comments: true
date: 2019-03-26 21:17:52
tags:
categories: ANDROID
---

#### 使用

#####   showAtLocation

````
 public void showAtLocation(View parent, int gravity, int x, int y) {
        mParentRootView = new WeakReference<>(parent.getRootView());
        showAtLocation(parent.getWindowToken(), gravity, x, y);
  }
````

* gravity 9种情况

  从 parent.getRootView()可以看到 parentview就是整个屏幕

  Gravity.NO_GRAVITY ：显示效果同 Gravity.LEFT | Gravity.TOP
  Gravity.LEFT：以屏幕左边中间位置为圆点
  Gravity.TOP：以屏幕顶部中间位置为参照物
  Gravity.RIGHT：以屏幕右侧中间位置为参照物
  Griavity.BOTTOM：以屏幕底部中间为参照物
  Gravity.LEFT | Gravity.TOP：以屏幕左上角为参照物
  Gravity.RIGHT | Gravity.TOP ：以屏幕右上角为参照物
  Gravity.LEFT | Gravity.BOTTOM ：以屏幕左下角为参照物
  Gravity.RIGHT | Gravity.BOTTOM ：以屏幕右下角为参照物
  x ：x < 0时，向左偏移， x >0 时，向右偏移

  y ：显示效果受gravity参数影响。当参数不带Gravity.BOTTOM时，y < 0，向上偏移， y > 0 ，向下偏移；当参数带有Gravity.BOTTOM时, y < 0,向下偏移，y > 0，向下偏移


  原文：https://blog.csdn.net/Justwen26/article/details/61621076 


 ##### showAsDropDown

```
public void showAsDropDown(View anchor, int xoff, int yoff) {
    showAsDropDown(anchor, xoff, yoff, DEFAULT_ANCHORED_GRAVITY);
}
```

* 弹窗会显示在anchor控件的正下方。