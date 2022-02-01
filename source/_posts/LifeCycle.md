---
title: LifeCycle
comments: true
date: 2020-08-02 20:47:59
tags:
categories: Jetpack
---



![](LifeCycle/2021-08-02_8.12_lifecycle.png)

<<<<<<< HEAD
LifeCycle应用

1. 解藕页面与组件
2. 解藕Service与组件
3. ProcessLifecycleOwner监听应用程序生命周期



##### 解藕页面与组件

```java
public class MyChronometer extends Chronometer implements LifecycleObserver {
    private long elapseTime;

    public MyChronometer(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    private void startMeter() {
        setBase(SystemClock.elapsedRealtime() - elapseTime);
        start();
    }
    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    private void stopMeter() {
        elapseTime = SystemClock.elapsedRealtime() - getBase();
        stop();
    }
}
```



```java
chronometer = (MyChronometer)findViewById(R.id.chronometer);
getLifecycle().addObserver(chronometer);
```



##### 解藕Service与组件
=======
>>>>>>> d7906d4909f312e25743be4a4c56ad1539df1ea6
