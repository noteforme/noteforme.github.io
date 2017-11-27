---
title: Include_Viewstub_merge
comments: true
date: 2017-11-23 17:26:55
tags:
categories: ANDROID

---

#  Include 用于解决重复的问题　




# Merge

解决多层布局嵌套的问题

```

  
    <?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/tv_top_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:text="项目进度"
        android:textColor="#8f8f8f"
        android:textSize="@dimen/dimen_12" />

    <TextView
        android:id="@+id/tv_detail"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/tv_title"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="@dimen/dimen_10"
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
            android:layout_marginRight="@dimen/dimen_10"
            android:layout_marginTop="-30dp"
            android:background="@color/white"
            android:paddingBottom="@dimen/dimen_12"
            android:paddingTop="@dimen/dimen_12">

            <RelativeLayout
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1">

                <include
                    android:id="@+id/ic_project_progress"
                    layout="@layout/include_invest" />
            </RelativeLayout>

            <View
                android:layout_width="0.5dp"
                android:layout_height="match_parent"
                android:background="@color/line_color" />
        
        </LinearLayout>
       

```
上面这种方式通过　　`findVIewById(R.id.ic_project_progress).findViewById(R.id.tv_top_title);`
就会报错　

看来一个页面多个地方共用还是不好用merge,id不好区分