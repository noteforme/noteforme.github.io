---
title: ConstraintLayout
comments: true
date: 2017-10-15 10:37:39
tags:
categories: "ANDROID"
---


## 常用布局方式

* 居中显示 : 四个方向添加约束
		 
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"

    android:id="@+id/constraint_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/white"
        app:layout_constraintBottom_toBottomOf="@+id/constraint_layout"
        app:layout_constraintEnd_toEndOf="@id/constraint_layout"
        app:layout_constraintStart_toStartOf="@id/constraint_layout"
        app:layout_constraintTop_toTopOf="@+id/constraint_layout">
    </Button>
</android.support.constraint.ConstraintLayout>

```





先看下大牛的　讲解[讲解](https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650824132&idx=1&sn=1cf09caa325d83de12b73c615fc9613e&chksm=80b7895ab7c0004c5cbb2175a3da302fc13d612762f56094f13b80e1334e7655dde1ad00083c&mpshare=1&scene=1&srcid=101591mRGp1DVSC425V6ZFCi&pass_ticket=ehu4gU6NQfzUXExFFvUCdyIfM4JyImAZha6dn4BZggZKAmpjNtpcb6XZgWicVQ7V#rd) 

这篇文章　图文并茂挺不错的
http://blog.chengyunfeng.com/?p=1030

对chain讲解详细
http://blog.csdn.net/zxt0601/article/details/72683379


https://developer.android.com/training/constraint-layout/index.html
https://developer.android.com/reference/android/support/constraint/ConstraintLayout.html