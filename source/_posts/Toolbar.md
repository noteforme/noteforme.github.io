---
title: Toolbar
comments: true
date: 2017-10-12 16:35:05
tags:
categories: ANDROID
---

#### 官网教程

 https://developer.android.com/training/appbar/setting-up.html?hl=zh-cn



##### toolbar整体设置

 ![tool设置](Toolbar/tool_color.png)

 参考:http://blog.csdn.net/a553181867/article/details/51336899


#####  颜色设置

  AppTheme下的设置

```
  <application
        android:name=".application.NBBApplication"
        android:allowBackup="true"
        android:icon="@mipmap/icon"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

```

* Toolbar颜色:
  `<item name="colorPrimary">@color/toolbar_color</item>`

* 状态栏设置 : 
  `<item name="android:windowTranslucentStatus">true</item>`
   在 API 19 及更高版本上，Toolbar 内容延伸至状态栏上去了, statebar把toolbar给覆盖了
   要解决这个问题就需要下面的属性

 *  android:fitsSystemWindows ="true"
     这个属性能保持与statebar一定 的padding,能解决上面windowTranslucentStatus的重叠问题

      参考: [亦枫](http://yifeng.studio/2017/02/19/android-statusbar/) 

* 去掉阴影线

     toolbar底部会有一条阴影线,通过AppBarLayout标签下的app:elevation="0dp"去掉



##### 自定义返回键

` mToolbar.setNavigationIcon(R.mipmap.ic_back);//自定义返回键`

或者这样设置 
 `app:navigationIcon="@mipmap/ic_back"`

 

##### toolbar.xml解读

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.AppBarLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:elevation="0dp">                    			 //设置仰角,0dp可以去掉底部的线

    <android.support.v7.widget.Toolbar
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:minHeight="?actionBarSize"
        android:theme="@style/ThemeOverlay.AppCompat.ActionBar"			//toolbar主题
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light">

        <TextView
            android:id="@+id/tb_title"
            style="@style/TextAppearance.AppCompat.Widget.ActionBar.Title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="标题"
            android:textColor="#333333"
            android:textSize="18sp" />
    </android.support.v7.widget.Toolbar>

</android.support.design.widget.AppBarLayout>

```








还有一种方式是在application设置，这样就不用集成baseActivity
https://www.diycode.cc/topics/783

#### menu设置

AppBarLayout下的 `app:theme="@style/toolbar_menu"`用于设置menu

##### menu字体颜色

需要设置 app:theme ,用 android:theme不起作用.

`<item name="android:actionMenuTextColor">@color/text_my_black</item>`

##### menu事件


    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_login, menu);
        menu.findItem(R.id.item_right_text).setTitle("注册"); //设置文字
        return true;
    }
    
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            
                break;
        }
        return super.onOptionsItemSelected(item);
    }

<https://blog.csdn.net/guyuealian/article/details/51721167>

#### 问题

使用toolbar过程中遇到了一些问题

* 问题 1

弹出键盘后 editext不见了，应该是toolbar被拉伸了


暂时解决方式:


         <activity
            android:name=".activity.WebActivity"
            android:hardwareAccelerated="true"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="adjustPan" />
https://github.com/CoolThink/StatusBarAdapt/issues/2


##### fragment使用toolbar
http://wuxiaolong.me/2015/12/21/fragmentToolbar/

##### 参考

郭霖公号:http://blog.csdn.net/james_shu/article/details/61661217

http://yifeng.studio/2016/10/12/android-toolbar/
http://www.bijishequ.com/detail/239876



####  三条线的颜色修改



```
<style name="DrawerArrowStyle" parent="Base.Widget.AppCompat.DrawerArrowToggle">
    <item name="spinBars">true</item>
    <item name="color">@android:color/white</item>
</style>
```

具体可以查看mineutil:PopupWindowActivity

* 溢出菜单三个点离右边的距离

 AppTheme中设置

```
 <item name="android:actionOverflowButtonStyle">@style/OverflowMenuStyle</item>

  <style name="OverflowMenuStyle" parent="Widget.AppCompat.Light.PopupMenu.Overflow">
        <item name="overlapAnchor">false</item>  <!--把该属性改为false即可使menu位置位于toolbar之下-->
        <item name="android:src">@mipmap/doc_ic_new_patient</item>
        <item name="android:paddingRight">15dp</item>
   </style>
```



<https://blog.csdn.net/hard_working1/article/details/77333893>



https://juejin.im/post/5e5898346fb9a07cd248d179

https://github.com/luckybilly/Gloading/blob/master/README-zh-CN.md