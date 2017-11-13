---
title: Toolbar
comments: true
date: 2017-10-12 16:35:05
tags:
categories: ANDROID
---

# 官网教程
 
 https://developer.android.com/training/appbar/setting-up.html?hl=zh-cn
在Best Practices for User Interface -> Adding the App Bar 





# Acitivty中添加工具栏配置

使用Toolbar方式的配置

* 使用库 v7 appcompat
* 确保Acitivty继承 AppCompatActivity

```
public class MyActivity extends AppCompatActivity {
  // ...
}

```
##  颜色设置

* Toolbar颜色:
`<item name="colorPrimary">@color/toolbar_color</item>`
* 状态栏设置 : 
`<item name="android:windowTranslucentStatus">true</item>`
 在 API 19 及更高版本上，Toolbar 内容延伸至状态栏上去了, statebar把toolbar给覆盖了
 要解决这个问题就需要下面的属性
 *  android:fitsSystemWindows ="true"
  这个属性能保持与statebar一定 的padding,能解决上面windowTranslucentStatus的重叠问题
 
 参考: [亦枫](http://yifeng.studio/2017/02/19/android-statusbar/) 
 
  
 
 
 
 





# 项目中导航栏 

http://yifeng.studio/2016/10/12/android-toolbar/
http://www.bijishequ.com/detail/239876


还有一种方式是在application设置，这样就不用集成baseActivity
https://www.diycode.cc/topics/783


#toolbar整体设置

 ![tool设置](Toolbar/tool_color.png)

 参考:http://blog.csdn.net/a553181867/article/details/51336899






