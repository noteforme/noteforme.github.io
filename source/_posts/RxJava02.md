---
title: RxJava02
comments: true
date: 2019-09-19 11:50:24
tags: RxJava
categories: ANDROID
---

**操作符**

- retryWhen

  `retryWhen`是收到`onError`后触发是否要重订阅的询问，而`repeatWhen`是通过`onComplete`触发。

https://blog.csdn.net/qq_35599978/article/details/80290252

https://www.jianshu.com/p/d135f19e045c

##### zip

专用于合并事件，该合并不是连接（连接操作符后面会说），而是两两配对，也就意味着，最终配对出的 `Observable` 发射事件数目只和少的那个相同。

##### `concat` 

单一的把两个发射器连接成一个发射器

##### `FlatMap` 

它可以把一个发射器 `Observable` 通过某种方法转换为多个 `Observables`,`flatMap` 并不能保证事件的顺序

如果需要保证，需要用到我们下面要讲的 `ConcatMap`

##### ConcatMap

##### doOnNext 

让订阅者在接收到数据之前可以做其他事情,获取到数据之前想先保存一下它

##### skip 

 skip(2)  跳过count个数开始接收

##### take 

最多接收count个数据

##### just

```java
Observable.just("1", "2", "3")
```

简单的发射器依次调用 `onNext()` 方法

##### Single

`Single` 只会接收一个参数，而 `SingleObserver` 只会调用 `onError()` 或者 `onSuccess()`

##### distinct

去重操作符，简单的作用就是去重

##### debounce

##### defer

每次订阅都会创建一个新的 `Observable`，并且如果没有被订阅，就不会产生新的

##### last 

操作符仅取出可观察到的最后一个值

##### merge 

##### window

按照实际划分窗口，将数据发送给不同的 `Observable`



##### empty() & never() & error()

> 1. empty() ： 直接发送 onComplete() 事件
> 2. never()：不发送任何事件
> 3. error()：发送 onError() 事件





https://www.jianshu.com/p/c08bfc58f4b6





