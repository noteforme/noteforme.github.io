---
title: Architecture
comments: true
date: 2018-06-14 09:31:59
tags:
categories: ANDROID
---

入门到深入

http://www.jcodecraeer.com/a/anzhuokaifa/2017/1024/8636.html



#### MVP基础

P M的复用

https://juejin.im/entry/5955e7166fb9a06bc23a8598

https://github.com/yaozs/YzsBaseActivity



##### 多个activity对p的复用

https://juejin.im/post/599ce8016fb9a0247e4255f4

Jetpack  以kotlin为基础U

####  介绍

https://developer.android.com/jetpack/

http://developers.googleblog.cn/2018/05/android-jetpack.html

[googlesamples](https://github.com/googlesamples/android-architecture-components)

https://codelabs.developers.google.com/codelabs/android-lifecycles/#1

架构学习从codelab开始吧!

https://blog.csdn.net/mq2553299/article/details/80445952

https://github.com/leon2017/GankJetpack

#### 项目参考

Java

https://github.com/googlesamples/android-architecture

http://mycommons.cn/2017/09/15/Android-Architecture-Components/#more





#### 组件化

https://mp.weixin.qq.com/s/8_8gGpkpO2QFNkWgSRBwIg

https://www.jianshu.com/p/6a50ef1ef45c

* 依赖冲突



  ```
  implementation 'com.github.ViksaaSkool:AwesomeSplash:v1.0.0'
  ```

​       

```
implementation ('com.github.ViksaaSkool:AwesomeSplash:v1.0.0') {    
    exclude group: 'com.android.support'
    exclude module: 'appcompat-v7'
    exclude module: 'support-v4'
}
```

```
gradlew app:dependencies
```

https://developer.android.com/studio/build/dependencies#duplicate_classes

https://developer.android.com/studio/build/manifest-merge?hl=zh-cn