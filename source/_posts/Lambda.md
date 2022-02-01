---
title: Lambda
comments: true
date: 2018-07-11 14:23:20
tags:
categories: JAVA
---



http://blog.oneapm.com/apm-tech/226.html

android 

https://maxwell-nc.github.io/android/retrolambda.html



https://cloud.tencent.com/developer/article/1526621



https://juejin.im/post/6844903668592934925



kotlin  lambda

https://juejin.im/post/6844903604613021703



Video

#### Java lambda 

函数式编程



https://www.bilibili.com/video/av54941486/

http://www.chilangedu.com/sectionq/2132352424



##### map

```
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

```
public interface Function<T, R> {
	    R apply(T t); //将T类型转为R
}
```



