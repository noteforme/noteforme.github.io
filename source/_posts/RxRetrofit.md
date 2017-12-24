---
title: RxRetrofit
comments: true
date: 2017-09-23 09:20:09
tags:
categories: ANDROID
---



# 　Retrofit

要实现类似这样的请求,用post方式怎么也不行
https://newsapi.org/v2/top-headlines?sources=financial-times&apiKey=e4f505f73a9f4ee99119ab33a19ab05e
最后还是感谢这位哥们http://wuxiaolong.me/2016/06/18/retrofits/


##  OFFICE 

http://square.github.io/retrofit/
https://github.com/square/retrofit

##  another

http://www.jianshu.com/p/308f3c54abdd
这篇文章不错，深入浅出
我还加了了提交Map参数的[Demo](https://gitee.com/huaiyi/RetrofitDemo.git) Demo


# 　RxJava2 Retorift2实例

##   RxJava Retrofit 简单使用

http://www.jianshu.com/p/56f15db86ed3

##   RxJava理解

-  subscribeOn() 指定上游发送事件的线程, observeOn　指定的是下游接收事件的线程

-  多次指定上游的线程只有第一次指定的有效,就是subscribeOn()只有第一次有效,其余忽略

-  可以多次指定下游线程,每调用一次observeOn(),下游的线程就会切换一次

Demo在　AndroidDemo -> RXLeran->RX基础->Rx线程切换

参考: http://www.jianshu.com/p/8818b98c44e2


-   Scheduler.io()

#  RxJava2学习

来自[鸿洋公众号的教程](http://www.10tiao.com/html/169/201709/2650823932/1.html) 

##  常用方式

https://github.com/ReactiveX/RxAndroid
https://github.com/amitshekhariitbhu/RxJava2-Android-Samples
https://github.com/rengwuxian/RxJavaSamples

#   Android architecture

##  Android架构

https://github.com/googlesamples/android-architecture
https://developer.android.com/topic/libraries/architecture/index.html


*　注意：android-architecture  todo-mvp-rxjava新版导入后会报错

Error:This Gradle plugin requires Studio 3.0 minimum

gradle.properties中：android.injected.build.model.only.versioned = 3

http://blog.csdn.net/suwenlai/article/details/78211563




