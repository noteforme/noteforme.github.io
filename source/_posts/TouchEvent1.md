---
title: TouchEvent1
comments: true
date:  2018-01-08 21:18:55
tags: TouchEvent
categories: VIEW
---

​																	**Android Touch Event**

#### Question

手机屏幕是由一个个像素点渲染而成的，不同的布局类似于幕布一层层叠上去形成了屏幕上显示的效果，The action is from  DOWN、MOVE、UP, How they formed while we used like longClick , touch, click?

a button on the listview item，why the listview item don't response while we click the button in a interview ?  but I cann't answer the key point.

multible listview or recycleview in a  Scrollview  ,how to resolve it?

Android provide NestedScrollView for us now,how does it work?

#### Answer

##### Image  clear

<img src="TouchEvent1/Screen_touch_event.png" alt="touch" style="zoom: 67%;" />



I found it very useful when i looked it.it make me have global view.and then I read Activity ViewGroup and View source code from framework. But I found some place could do better ,so I draw another Image.

<img src="TouchEvent1/ACTION_DOWN No childView.png" alt="action down" style="zoom:67%;" />





<img src="TouchEvent1/ACTION_DOWN.png" alt="down" style="zoom:67%;" />





<img src="TouchEvent1/ACTION_MOVE UP.png" alt="move" style="zoom:67%;" />



开发中常用的事件分发

ListView　

item 上有Button ,item上的onItemClick事件得不到响应，可以看看官方[ViewGroup](https://developer.android.com/reference/android/view/ViewGroup.html#attr_android:descendantFocusability) 描述

item Button

> 页面搜索　android:descendantFocusability
>
> android:descendantFocusability属性共有三个取值，分别为
> beforeDescendants：viewgroup会优先其子类控件而获取到焦点
> afterDescendants：viewgroup 只有当其子类控件不需要获取焦点时才获取焦点
> blocksDescendants：viewgroup 会覆盖子类控件而直接获得焦点

原因 : 在View `onTouchEvent(MotionEvent event) `中,只要view可点击就返回true,事件就被消费掉了,
    set isClickable = true  才可以收到 action move up事件



事件分发

https://juejin.im/post/6862254703837708302

https://www.jianshu.com/p/d82f426ba8f7

https://www.jianshu.com/p/e99b5e8bd67b

https://www.jianshu.com/p/5279e887841b

滑动冲突

https://blog.csdn.net/gdutxiaoxu/article/details/52939127

https://www.jianshu.com/p/982a83271327

https://juejin.im/user/4336129588334958/posts



