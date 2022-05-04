---
title: LifeCycle
comments: true
date: 2020-08-02 20:47:59
tags:
categories: Jetpack
---



![](LifeCycle/2021-08-02_8.12_lifecycle.png)LifeCycle应用

1. 解藕页面与组件
2. 解藕Service与组件
3. ProcessLifecycleOwner监听应用程序生命周期



##### 解藕页面与组件

```java

```



```java
chronometer = (MyChronometer)findViewById(R.id.chronometer);
getLifecycle().addObserver(chronometer);
```



##### 解藕Service与组件

https://blog.csdn.net/vitaviva/article/details/121224946
