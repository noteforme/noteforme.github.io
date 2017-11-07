---
title: RecyclerView使用
date: 2017-07-17 11:30:18
tags:
categories: "ANDROID"
comments: true

---

# 先来介绍官方的Demo

 https://github.com/googlesamples/android-RecyclerView
    

和LIstView其他类似
主要是可以设置横竖屏　        mLayoutManager = new LinearLayoutManager(this);

**注意**：如果收到的字段是 int类型的　直接setText()会闪退
并爆出类似这种错误
> android.content.res.Resources$NotFoundException: String resource ID Fatal Exception in Main

早一点搜就好了，浪费半小时，找问题
https://stackoverflow.com/questions/15191092/android-content-res-resourcesnotfoundexception-string-resource-id-fatal-except


#常用方式

## 下拉刷新和加载更多
  这个是我们最常见的使用方式，可以根据getItemViewType判断item类型来添加头和尾
  例如这个类  FundSerialsActivityNew
##  使用　装饰器模式
 

 



http://blog.csdn.net/lmj623565791/article/details/51854533
http://blog.csdn.net/qibin0506/article/details/49716795



#问题
使用recycleView中会遇到一些问题,总结一下

## 下拉刷新 滑动列表 crash
 
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

参考: http://blog.csdn.net/weixiao_812/article/details/78138075
https://allen218.github.io/2016/06/02/RecyclerView%E4%B8%8B%E6%8B%89%E5%88%B7%E6%96%B0%E6%97%B6%E5%BF%AB%E9%80%9F%E6%BB%91%E5%8A%A8%E5%B4%A9%E6%BA%83%E7%9A%84%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/

# 　Item分类

前面添加header, footer就用到了item分类
reclverview也有很多分类方式

[多adapter分类](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0810/3282.html) 
使用adapter　组合设计模式，进行组装，代码简洁，比较好操作


这种对用到的页面操作比较简单，super里面想放哪些就可以，就是itemviewtype方法实现比较复杂，判断容易出错
https://github.com/luizgrp/SectionedRecyclerViewAdapter


# listview recleview通用

 listview recleview通用，并且添加头尾的开源项目

https://github.com/liaoinstan/SpringView


