---
title: ViewDynamic
comments: true
date: 2018-07-17 20:25:26
tags:
categories:	ANDROID
---



#### 动态添加布局

##### LinearLayout添加

* 父容器

  ```
  <LinearLayout
      android:id="@+id/ll_plan_one"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
  </LinearLayout>
  ```

* 子布局

  ```
  View view1 = inflate.inflate(R.layout.layout_animation, null);
  View view2 = inflate.inflate(R.layout.ic_hpan_suger_press_y, null);
  View view3 = inflate.inflate(R.layout.ic_hpan_suger_press_y, null);
  ```

* 添加进容器

  ```
    llVertical.addView(view1);
    llVertical.addView(view2);
    llVertical.addView(view3);
  ```

* 添加同一个子布局会**报错**

  ```
  llVertical.addView(view1);
  llVertical.addView(view1);
  ```

  > ​    Caused by: java.lang.IllegalStateException: The specified child already has a parent. You must call removeView() on the child's parent first.