---
title: TAB_VIEWPAGER
comments: true
date: 2017-12-26 15:32:39
tags: TabLayout
categories: ANDROID
---

https://developer.android.com/reference/com/google/android/material/tabs/TabLayout

####  ViewPager

#####  TabLayout方式

* 动态

```
TabLayout tabLayout = ...;
 tabLayout.addTab(tabLayout.newTab().setText("Tab 1"));
 tabLayout.addTab(tabLayout.newTab().setText("Tab 2"));
 tabLayout.addTab(tabLayout.newTab().setText("Tab 3"));
```

* 静态

  ```
   <com.google.android.material.tabs.TabLayout
           android:layout_height="wrap_content"
           android:layout_width="match_parent">
  
       <com.google.android.material.tabs.TabItem
               android:text="@string/tab_text"/>
  
       <com.google.android.material.tabs.TabItem
               android:icon="@drawable/ic_android"/>
  
   </com.google.android.material.tabs.TabLayout>
   
  ```





##### 滑动TAB

```
<androidx.viewpager.widget.ViewPager
     android:layout_width="match_parent"
     android:layout_height="match_parent">

     <com.google.android.material.tabs.TabLayout
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         android:layout_gravity="top" />

 </androidx.viewpager.widget.ViewPager>
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



* 默认设置

  ```
  mTabLayout.getTabAt(roomType).select();
  viewpager.setCurrentItem(1);
  ```

  https://segmentfault.com/a/1190000008753052


#####  自定义TabItem

* tab_item.xml

  ```
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="wrap_content">
  
      <TextView
          android:id="@+id/tv_item_title"
          android:layout_width="40dp"
          android:layout_height="40dp"
          android:layout_gravity="center_horizontal"
          android:background="@drawable/bg_tab_item_select"
          android:gravity="center"
          android:textColor="@drawable/bg_tab_item_select_txt"
          android:textSize="@dimen/text_size_14" />
  
      <View
          android:id="@+id/view_line"
          android:layout_width="0dp"
          android:layout_height="1dp"
          android:layout_gravity="center_vertical"
          android:layout_marginLeft="@dimen/dimen_10"
          android:layout_weight="1"
          android:background="@android:color/darker_gray" />
  </LinearLayout>
  
  ```


```
   tabLayout = findViewById(R.id.tb_layout);
        for (int i = 0; i < tabTitles.length; i++) {
            tabLayout.addTab(tabLayout.newTab());
        }

        for (int i = 0; i < tabTitles.length; i++) {
            TabLayout.Tab tab = tabLayout.getTabAt(i);
            tab.setCustomView(R.layout.tab_item);
            TextView tvItemTitle = tab.getCustomView().findViewById(R.id.tv_item_title);
            tvItemTitle.setText(tabTitles[i]);
            if (i == tabTitles.length - 1) {
                tab.getCustomView().findViewById(R.id.view_line).setVisibility(View.GONE);
            }
        }
```



https://www.jianshu.com/p/ed129686f2cc

https://blog.csdn.net/android_zhengyongbo/article/details/74726176

#####  修改指示器颜色

https://codeday.me/bug/20170629/35704.html

#####  设置Tab选择背景



<https://blog.csdn.net/jdsjlzx/article/details/52171075>

 ![tabLayout_20190603110418](TAB_VIEWPAGER\tabLayout_20190603110418.png)

首先设置 

* tabGravity fill模式 center是居中

* 通过layer-list设置间距

  ```
  <?xml version="1.0" encoding="utf-8"?>
  <selector xmlns:android="http://schemas.android.com/apk/res/android">
      <item android:state_selected="true">
          <layer-list>
              <item>
                  <shape>
                      <solid android:color="@android:color/transparent" />
                  </shape>
  
              </item>
              <item android:left="@dimen/dimen_10" android:right="@dimen/dimen_10">
                  <shape android:shape="rectangle">
                      <corners android:radius="3dp" />
                      <solid android:color="#ffa2cb79" />
                  </shape>
              </item>
          </layer-list>
  
      </item>
  
      <item >
          <layer-list>
              <item>
                  <shape>
                      <solid android:color="@android:color/transparent"></solid>
                  </shape>
              </item>
  
              <item android:left="@dimen/dimen_10" android:right="@dimen/dimen_10">
                  <shape android:shape="rectangle">
                      <corners android:radius="3dp" />
                      <solid android:color="#ffdddddd" />
                  </shape>
              </item>
          </layer-list>
  
  
      </item>
  
  </selector>
  ```

  

#### ViewPager2

#####  migration

https://developer.android.com/training/animation/screen-slide-2#kotlin

https://developer.android.com/training/animation/vp2-migration

https://github.com/android/views-widgets-samples/tree/master/ViewPager2



```
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/view_pater2"
    android:layout_width="match_parent"
    android:layout_height="0dp"
    android:layout_marginTop="27dp"
    app:layout_constraintBottom_toBottomOf="parent" />
```

```
view_pater2.adapter = ScreenSlidePagerAdapter(this)
```

```
private inner class ScreenSlidePagerAdapter(fa: Fragment) : FragmentStateAdapter(fa) {
    override fun getItemCount(): Int = 2
    override fun createFragment(position: Int): Fragment{
        return if (position==1){
            BodyDataFragment.newInstance()
        }else{
            SleepFragment.newInstance()
        }
    }
}
```