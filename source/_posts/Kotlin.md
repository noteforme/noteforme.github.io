---
title: Kotlin
comments: true
date: 2019-05-15 15:32:51
tags:
categories: Kotlin
---



##### FindViewById

​	Kotlin不用findViewById 注意在Fragmet中 需要在onViewCreated后使用

​	 https://blog.csdn.net/hust_twj/article/details/80290362 

* The reason why we ignore findviewbyId

   https://antonioleiva.com/kotlin-android-extensions/ 

?  

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

!!

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



##### null or empty

```
    data = " " // this is a text with blank space 

    println(data.isNullOrBlank()?.toString())  //true
    println(data.isNullOrEmpty()?.toString())  //false
```



##### Fragment

```
override fun onAttach(context: Context) {
    super.onAttach(context)
    if (context is IPatientListener) {
        mListener = context
    } else {
        throw RuntimeException(context!!.toString() + " must implement IServiceListener")
    }
}
```





https://developer.android.com/samples/?language=kotlin

https://developer.android.com/kotlin/get-started

https://www.jianshu.com/p/9fb9a1ab6c31

https://juejin.im/post/5aa64556f265da238c3a51d3



#### 高级用法

<https://www.cnblogs.com/Jetictors/p/9225557.html>



#####  coroutines

https://www.kotlincn.net/docs/reference/coroutines/coroutines-guide.html



##### by

https://blog.csdn.net/wzgiceman/article/details/82689135



oprator

https://zhuanlan.zhihu.com/p/26546977