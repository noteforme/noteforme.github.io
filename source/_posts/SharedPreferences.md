---
title: SharedPreferences
comments: true
date: 2019-11-06 14:50:12
tags:
categories: SOURCE
---





![](https://s5.51cto.com/oss/202110/15/d91bb33922311055194a101ffe69424a.png)



#### Question

##### 问题1

sp初始化 是读取所有的文件，还是指定文件的数据。

##### 问题2

sp读取文件后会把数据保存到map中，如果后续我pusString() 数据了，还需要重新读取文件吗.



#### 启动变慢

sp可能导致启动变慢 :  

```java
private void startLoadFromDisk() {
    synchronized (mLock) {
        mLoaded = false;
    }
    new Thread("SharedPreferencesImpl-load") {
        public void run() {
            loadFromDisk();
        }
    }.start();
}
```

1. mLock是异步操作
2. 如果启动的时候，主线程读取，会导致启动变慢

```java
@Override
public boolean getBoolean(String key, boolean defValue) {
    synchronized (mLock) {
        awaitLoadedLocked();
        Boolean v = (Boolean)mMap.get(key);
        return v != null ? v : defValue;
    }
}
```



```java
@GuardedBy("mLock")
private void awaitLoadedLocked() {
    if (!mLoaded) {
        // Raise an explicit StrictMode onReadFromDisk for this
        // thread, since the real read will be in a different
        // thread and otherwise ignored by StrictMode.
        BlockGuard.getThreadPolicy().onReadFromDisk();
    }
    while (!mLoaded) {
        try {
            mLock.wait();
        } catch (InterruptedException unused) {
        }
    }
    if (mThrowable != null) {
        throw new IllegalStateException(mThrowable);
    }
}
```

所有 getXXX() 方法都是同步的，在主线程调用 get 方法，必须等待 SP 加载完毕，会导致主线程阻塞，下面的代码，我相信小伙伴们并不陌生。



#### commit vs apply

commit ： 是同步

Apply : 异步



https://yjy239.github.io/2020/05/04/android-chong-xue-xi-lie-sharedpreferences-yuan-ma-jie-xi/

https://www.51cto.com/article/685850.html



解决SharedPreferences缺陷，微信MMKV原理分析

https://www.bilibili.com/video/BV1HF411b7H1?p=6&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1ih411a7KG?p=1



#### DataStore



SharedPreferences迁移

https://www.hi-dhl.com/2020/10/19/jetpack/11-DataStore/





![2022-05-27_4.07.44](SharedPreferences/2022-05-27_4.07.44.png)
