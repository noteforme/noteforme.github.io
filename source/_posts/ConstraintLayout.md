---
title: ConstraintLayout
comments: true
date: 2017-10-15 10:37:39
tags:
categories: "ANDROID"
---

#  office

https://developer.android.com/reference/android/support/constraint/ConstraintLayout.html

##  Here is the list of available constraints (Fig. 2):

layout_constraintLeft_toLeftOf
layout_constraintLeft_toRightOf
layout_constraintRight_toLeftOf
layout_constraintRight_toRightOf
layout_constraintTop_toTopOf
layout_constraintTop_toBottomOf
layout_constraintBottom_toTopOf
layout_constraintBottom_toBottomOf
layout_constraintBaseline_toBaselineOf
layout_constraintStart_toEndOf
layout_constraintStart_toStartOf
layout_constraintEnd_toStartOf
layout_constraintEnd_toEndOf

They all take a reference id to another widget, or the parent (which will reference the parent container, i.e. the ConstraintLayout):

```
<Button android:id="@+id/buttonB" ...
                 app:layout_constraintLeft_toLeftOf="parent" />
```

##  Margins

If side margins are set, they will be applied to the corresponding constraints (if they exist) (Fig. 3), enforcing the margin as a space between the target and the source side. The usual layout margin attributes can be used to this effect:

android:layout_marginStart
android:layout_marginEnd
android:layout_marginLeft
android:layout_marginTop
android:layout_marginRight
android:layout_marginBottom
Note that a margin can only be positive or equals to zero, and takes a Dimension.

Margins when connected to a GONE widget
When a position constraint target's visibility is View.GONE, you can also indicate a different margin value to be used using the following attributes:

layout_goneMarginStart
layout_goneMarginEnd
layout_goneMarginLeft
layout_goneMarginTop
layout_goneMarginRight
layout_goneMarginBottom

Centering positioning and bias
A useful aspect of ConstraintLayout is in how it deals with "impossible" constrains. For example, if we have something like:

```
           <android.support.constraint.ConstraintLayout ...>
             <Button android:id="@+id/button" ...
                 app:layout_constraintLeft_toLeftOf="parent"
                 app:layout_constraintRight_toRightOf="parent/>
           </>
         
```



#  常用布局方式

##   居中显示 : 四个方向添加约束
		 
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
