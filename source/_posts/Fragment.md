---
title: Fragment
comments: true
date: 2017-10-05 19:54:50
tags:
categories:  ANDROID
---

https://developer.android.com/reference/android/app/Fragment.html

https://wizardforcel.gitbooks.io/w3school-android/content/77.html
https://juejin.im/post/5901b564570c35005804424b

#  Fragment包的区别

fragment有两种继承方式
android.support.v4.app.Fragment和android.app.Fragment

下面作者　总结的不错　干脆直接用了

> android.app.Fragment 兼容的最低版本是android:minSdkVersion="11" 即3.0版
android.support.v4.app.Fragment 兼容的最低版本是android:minSdkVersion="4" 即1.6版

> android.app.Fragment使用 (ListFragment)getFragmentManager().findFragmentById(R.id.userList) 获得 ，继承Activity
android.support.v4.app.Fragment使用 (ListFragment)getSupportFragmentManager().findFragmentById(R.id.userList) 获得 ，需要继承android.support.v4.app.FragmentActivity


Android Support兼容包

Support Library
我们都知道Android一些SDK比较分裂，为此google官方提供了Android Support Library package 系列的包来保证高版本sdk开发的向下兼容性, 所以你可能经常看到v4，v7，v13这些数字，首先我们就来理清楚这些数字的含义，以及它们之间的区别。

1.support-v4
用在API lever 4(即Android 1.6)或者更高版本之上。它包含了相对更多的内容，而且用的更为广泛，例如：Fragment，NotificationCompat，LoadBroadcastManager，ViewPager，PageTabAtrip，Loader，FileProvider 等
Gradle引用方法：

compile 'com.android.support:support-v4:21.0.3'
2.support-v7
这个包是为了考虑API level 7(即Android 2.1)及以上版本而设计的，但是v7是要依赖v4这个包的，v7支持了Action Bar以及一些Theme的兼容。
Gradle引用方法:

compile 'com.android.support:appcompat-v7:21.0.3'
3.support-v13
这个包的设计是为了API level 13(即Android 3.2)及更高版本的，一般我们都不常用，平板开发中能用到，这里就不过多介绍了。

作者：天天想念
链接：http://www.jianshu.com/p/db28adde1c39

#  Fragment懒加载

Fragment和ViewPager一起使用会有个预加载机制，会把旁白的Fragment的生命周期方法
前半段先执行，然后执行自身的生命周期方法
