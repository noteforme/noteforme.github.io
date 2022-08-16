---
title: kotlin-inline
date: 2022-07-24 11:54:53
categories: Kotlin
---

![20220724115333](kotlin-inline/20220724115333.jpg)



#### inline 

4:00分钟 

```kotlin
fun main() {
    hello {
        println("Bye")
    }
}
```



##### 不加inline

```kotlin
fun hello(postAction: () -> Unit) {
    println("Hello!")
    postAction()
}
```

实际编译结果大致

```kotlin
fun main(){
	val post. = object: Function0<Unit>{
    override fun nvoke(){
      return prinltn("Bye!")
    }
  }
}
```

如果在for(i in 1.. 100) 就会创建很对个对象



##### 加 inline

```kotlin
fun hello(postAction: () -> Unit) {
    println("Hello!")
    postAction()
}
```

不仅把函数内连过来,也会把它内部的函数的类型的参数 ，lambda表达式也内联过来.

//编译代码 类似

```kotlin
fun main(){
  println("Hello!");
  println("Bye!")
}
```

这样就不存在，对象的创建





noinline

crossinline







https://www.bilibili.com/video/BV1kz4y1f7sf

