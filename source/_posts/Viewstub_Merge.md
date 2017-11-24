---
title: Include_Viewstub_merge
comments: true
date: 2017-11-23 17:26:55
tags:
categories: ANDROID

---

# Merge

解决多层布局嵌套的问题

```

  
     <?xml version="1.0" encoding="utf-8"?>
      <merge xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="wrap_content">

        <TextView
         android:id="@+id/tv_progress"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:layout_marginTop="@dimen/dimen_16"
         android:text="项目进度"
         android:textColor="#8f8f8f"
         android:textSize="@dimen/dimen_12" />

        <TextView
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:layout_below="@+id/tv_progress"
         android:text="60%"
         android:textColor="#2c97f4"
         android:textSize="@dimen/text_size_14" />
</merge>

```
 

```
     
       <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="@dimen/dimen_10"
        android:layout_marginRight="@dimen/dimen_10">

        <RelativeLayout
            layout="@layout/include_invest"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1">

            <include layout="@layout/include_invest" />
        </RelativeLayout>

        <RelativeLayout
            layout="@layout/include_invest"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1">

            <include layout="@layout/include_invest" />
        </RelativeLayout>

        <RelativeLayout
            layout="@layout/include_invest"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1">

            <include layout="@layout/include_invest" />
        </RelativeLayout>

    </LinearLayout>

```
