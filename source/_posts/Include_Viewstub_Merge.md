---
title: Include_Viewstub_merge
comments: true
date: 2017-11-23 17:26:55
tags:
categories: ANDROID

---

##  Include 用于解决重复的问题　

AndroidStudio 选择Tools -> Android -> Layout Inspector 可以查看层级

例如：

```
  <RelativeLayout
            android:id="@+id/rl_repair"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp">

            <include layout="@layout/item_service" />
        </RelativeLayout>

        <RelativeLayout
            android:id="@+id/rl_advice"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp">

            <include layout="@layout/item_service" />
        </RelativeLayout>
```

分别给include内控件赋值

```
  int[] intRelative = ItemUtil.getServiceRelativeLayout();
        int[] iconInt = ItemUtil.getServiceIcon();
        String[] title = ItemUtil.getServiceTitle();
        String[] describe = ItemUtil.getServiceDescribe();
        for (int i = 0; i < intRelative.length; i++) {
            RelativeLayout rlView = view.findViewById(intRelative[i]);
            ImageView ivIcon = rlView.findViewById(R.id.iv_finance);
            ivIcon.setImageResource(iconInt[i]);
            TextView tvTitle = rlView.findViewById(R.id.tv_title);
            TextView tvDescribe = rlView.findViewById(R.id.tv_describe);
            tvTitle.setText(title[i]);
            tvDescribe.setText(describe[i]);
            rlView.setOnClickListener(this);
        }
```










## Merge

解决多层布局嵌套的问题

* include_invest.xml
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
 merge标签相当于没有，直接融入父布局
看来一个页面多个地方共用还是不好用merge,id不好区分



##  ViewStub使用

```
	  <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="@dimen/dimen_10"
            android:background="@color/white"
            android:paddingBottom="@dimen/dimen_15"
            android:paddingTop="@dimen/dimen_15">

            <TextView
                android:id="@+id/tv_project_name"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_marginLeft="@dimen/dimen_12"
                android:textColor="#212121"
                android:textSize="@dimen/text_size_15" />

            <ViewStub
                android:id="@+id/vs_isnew"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentRight="true"
                android:layout_centerVertical="true"
                android:layout_marginRight="@dimen/dimen_12"
                android:layout="@layout/vs_project_newleader" />
        </RelativeLayout>
```
* 使用

      ViewStub vsEmpty = findViewById(R.id.vs_isnew);
  
-  visible 

  调用ViewStub 的setVisibility() 方法来显示这个View,这样的话这个被加载的view应该是固定的

  
  vsEmpty.setVisible(View.Gone)
  
  inflate

  通过ViewStub 的inflate() 方法来显示这个View,可以获取到inflate 进去的View，这样可以动态加载view不同的内容

  ```
       View view = vsEmpty.inflate();
       
       TextView text = view.findViewById(R.id.tv_txt);
       text.setText("测试");
  ```

  判断ViewStub 是否inflate()
  
 * 方法1 :

			ViewStub viewStub=  (ViewStub) findViewById(R.id.viewstub);
		if(viewStub!=null){
  		　  viewStub.inflate();
		}
* 方法2（推荐）: 
		//初始化，不必每次都调用
		ViewStub viewStub=  (ViewStub) findViewById(R.id.viewstub);
          
if(viewStub.getParent() !=null){
    viewStub.inflate();
}

    http://blog.csdn.net/qq_35956194/article/details/79080703
    https://droidyue.com/blog/2016/09/11/using-viewstub-in-android-to-improve-layout-performance/

  参考
https://developer.android.com/reference/android/view/ViewStub.html
https://developer.android.com/training/improving-layouts/loading-ondemand.html


https://jaysdev.github.io/2017/07/09/ViewStub%E4%BD%BF%E7%94%A8/

* ​