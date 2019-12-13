---
title: AndroidViewDispatch02
comments: true
date:  2018-01-08 21:18:55
tags: EventDispatch
categories: VIEW
---

**Android事件分发机制 实际应用**


开发中常用的事件分发

##  ListView　

- item Button

item 上有Button ,item上的onItemClick事件得不到响应，可以看看官方[ViewGroup](https://developer.android.com/reference/android/view/ViewGroup.html#attr_android:descendantFocusability) 描述

页面搜索　android:descendantFocusability
>
android:descendantFocusability属性共有三个取值，分别为
beforeDescendants：viewgroup会优先其子类控件而获取到焦点
afterDescendants：viewgroup 只有当其子类控件不需要获取焦点时才获取焦点
blocksDescendants：viewgroup 会覆盖子类控件而直接获得焦点

- 原因 : 在View `onTouchEvent(MotionEvent event) `中,只要view可点击就返回true,事件就被消费掉了,
                 

https://www.diycode.cc/topics/352






- 添加头部banner
  https://www.jianshu.com/p/5279e887841b

- https://blog.csdn.net/gdutxiaoxu/article/details/52939127

  https://www.jianshu.com/p/982a83271327