---
title: LifeCycle
comments: true
date: 2020-08-02 20:47:59
tags:
categories: Jetpack
---



https://developer.android.google.cn/topic/libraries/architecture/lifecycle

![The ViewModel survives an activity recreation and only the onCleared method is called when the activity is finished.](https://developer.android.google.cn/static/codelabs/android-lifecycles/img/1d42e8efcb42ff58.png)



![](LifeCycle/2021-08-02_8.12_lifecycle.png)LifeCycle应用

1. 解藕页面与组件
2. 解藕Service与组件
3. ProcessLifecycleOwner监听应用程序生命周期





![Diagram of lifecycle states](https://developer.android.google.cn/static/images/topic/libraries/architecture/lifecycle-states.svg)





##### 解藕页面与组件

```java
chronometer = (MyChronometer)findViewById(R.id.chronometer);
getLifecycle().addObserver(chronometer);
```



##### 解藕Service与组件

https://blog.csdn.net/vitaviva/article/details/121224946



#####  SavedStateHandle

直接给viewmodel传值



https://medium.com/mobile-app-development-publication/passing-intent-data-to-viewmodel-711d72db20ad



##### CodeLab

https://developer.android.com/codelabs/android-lifecycles#4
