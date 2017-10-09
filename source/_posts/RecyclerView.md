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