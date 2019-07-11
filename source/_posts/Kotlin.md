---
title: Kotlin
comments: true
date: 2019-05-15 15:32:51
tags:
categories: ANDROID
---



##### FindViewById

Kotlin不用findViewById 

注意在Fragmet中 需要在onViewCreated后使用

<https://blog.csdn.net/hust_twj/article/details/80290362>



* ?  

```
//kotlin:
a?.foo()

//相当于java:
if(a!=null){
  a.foo();
}else{
   null 
}
```

-  !!

  ```
  //kotlin:
  a!!.foo()
  
  //相当于java:  
  if(a!=null){
    a.foo();
  }else{
    throw new KotlinNullPointException();
  }
  ```



https://developer.android.com/samples/?language=kotlin

https://developer.android.com/kotlin/get-started

https://www.jianshu.com/p/9fb9a1ab6c31

https://juejin.im/post/5aa64556f265da238c3a51d3



#### 高级用法

<https://www.cnblogs.com/Jetictors/p/9225557.html>

