---
title: TabLayout
comments: true
date: 2017-12-26 15:32:39
tags: TabLayout
categories: ANDROID
---

TabLayout属于 Material Design

https://developer.android.com/reference/android/support/design/widget/TabLayout.html

# 滑动TAB

```
	   <android.support.v4.view.ViewPager
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <android.support.design.widget.TabLayout
            android:id="@+id/tab_layout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="top"
            app:tabTextAppearance="@style/TabLayoutNews"
            app:tabMinWidth="100dp"
            app:tabTextColor="@color/text_black" />

    </android.support.v4.view.ViewPager>

```

# 选择TAB
