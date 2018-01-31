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
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <android.support.design.widget.TabLayout
        android:id="@+id/tab_vp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode="fixed"
        app:tabSelectedTextColor="#2e97f3"
        app:tabTextColor="#2e97f3" />

    <android.support.v4.view.ViewPager
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>

```

设置Tab

```

        TabLayout tabVp = rootView.findViewById(R.id.tab_vp);
        viewPager = rootView.findViewById(R.id.view_pager);
          framgents = new LinkedHashMap<>();
        framgents.put("项目详情", introFragment);
        framgents.put("相关图片", imageFragment);
        framgents.put("投资记录", rankFragment);

        titles = new LinkedList<>();
        titles.addAll(framgents.keySet());

        viewPager.setAdapter(new MyPagerAdapter(getFragmentManager()));
        tabVp.setupWithViewPager(viewPager);
```







# 选择TAB