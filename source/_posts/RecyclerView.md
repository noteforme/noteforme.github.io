---
title: RecyclerView使用
date: 2017-07-17 11:30:18
tags:
categories: "ANDROID"
comments: true

---



####  RecyclerView

#####  基本使用

```
LinearLayoutManager layoutManager = new LinearLayoutManager(getActivity());
layoutManager.setOrientation(LinearLayoutManager.VERTICAL); //默认竖直布局
mRecyclerView.setLayoutManager(layoutManager);
```

 先来介绍官方的Demo https://github.com/googlesamples/android-RecyclerView

  http://blog.csdn.net/lmj623565791/article/details/51854533
http://blog.csdn.net/qibin0506/article/details/49716795

##### 问题
使用recycleView中会遇到一些问题,总结一下

-  问题1
    下拉刷新 滑动列表 crash

使用下拉刷新的时候 往上滑动item会出现这样的错误

```
 E/CrashReport: sys default last handle start!
 FATAL EXCEPTION: main
  Process: com.huifu, PID: 22865
  java.lang.IndexOutOfBoundsException: Inconsistency detected. Invalid item position 4(offset:4).state:10
  at android.support.v7.widget.RecyclerView$Recycler.tryGetViewHolderForPositionByDeadline(RecyclerView.java:5504)
  at android.support.v7.widget.GapWorker.prefetchPositionWithDeadline(GapWorker.java:282)
  at android.support.v7.widget.GapWorker.flushTaskWithDeadline(GapWorker.java:336)
  at android.support.v7.widget.GapWorker.flushTasksWithDeadline(GapWorker.java:349)
  at android.support.v7.widget.GapWorker.prefetch(GapWorker.java:356)
  at android.support.v7.widget.GapWorker.run(GapWorker.java:387)
  at android.os.Handler.handleCallback(Handler.java:739)
  at android.os.Handler.dispatchMessage(Handler.java:95)
  at android.os.Looper.loop(Looper.java:148)
  at android.app.ActivityThread.main(ActivityThread.java:5417)
  at java.lang.reflect.Method.invoke(Native Method)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:726)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:616)

```
 刷新之后数据清空，再滑动数据和item不一致, 情况数据应该在拿到数据之后, 使用下面这种方式解决问题，

```
    @Override
    public void onRefresh() {
//        if (!mProjectList.isEmpty()) {
//            mProjectList.clear();
//        }
        start = 0;
        loadData();
    }
//拿到数据后

 public void onSuccess(List<Project> projects) {
                        swipeInvest.setRefreshing(false);
                        isloading = false;
                        if (start == 0) mProjectList.clear();
 }

```

参考:  http://blog.csdn.net/weixiao_812/article/details/78138075
 [下拉刷新时快速滑动崩溃的问题解决](https://allen218.github.io/2016/06/02/RecyclerView%E4%B8%8B%E6%8B%89%E5%88%B7%E6%96%B0%E6%97%B6%E5%BF%AB%E9%80%9F%E6%BB%91%E5%8A%A8%E5%B4%A9%E6%BA%83%E7%9A%84%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/)  

-  问题2

界面不能展示 ,和ListView  不同的是RecyclerView不同,没设置下面的参数是不显示的
```
        mLayoutManager = new LinearLayoutManager(getActivity());
        rcvRrecord.setLayoutManager(mLayoutManager);
```
如果还是没有就看看 recycleview布局是否显示

##### Item分类

前面添加header, footer就用到了item分类
reclverview也有很多分类方式


- 使用adapter　组合设计模式，进行组装，代码简洁，比较好操作
  [多adapter分类](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0810/3282.html) 

https://github.com/luizgrp/SectionedRecyclerViewAdapter
http://blog.csdn.net/wzlyd1/article/details/52292548
https://github.com/luizgrp/SectionedRecyclerViewAdapter
 复杂布局: https://github.com/385841539/RecycleviewStaggered

 竖直嵌套水平:https://github.com/drakeet/MultiType/issues/67

##### 分割线

https://www.jianshu.com/p/4eff036360da

https://www.jianshu.com/p/b2ef4f8e859f



#### 字符排序

https://github.com/YoKeyword/IndexableRecyclerView