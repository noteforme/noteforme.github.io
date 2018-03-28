---
title: RxJava
comments: true
date: 2018-03-28 17:58:39
tags: RxJava
categories: ANDROID
---

## RxJava Retrofit 简单使用

http://www.jianshu.com/p/56f15db86ed3

## RxJava理解

- subscribeOn() 指定上游发送事件的线程, observeOn　指定的是下游接收事件的线程
- 多次指定上游的线程只有第一次指定的有效,就是subscribeOn()只有第一次有效,其余忽略
- 可以多次指定下游线程,每调用一次observeOn(),下游的线程就会切换一次

Demo在　AndroidDemo -> RXLeran->RX基础->Rx线程切换

参考: http://www.jianshu.com/p/8818b98c44e2

- Scheduler.io()

# RxJava2学习

来自[鸿洋公众号的教程](http://www.10tiao.com/html/169/201709/2650823932/1.html) 



ZIP使用

## 常用方式

https://github.com/ReactiveX/RxAndroid
https://github.com/amitshekhariitbhu/RxJava2-Android-Samples
https://github.com/rengwuxian/RxJavaSamples