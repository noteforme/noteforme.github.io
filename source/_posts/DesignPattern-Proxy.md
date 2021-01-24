---
title: DesignPattern_Proxy
comments: true
date: 2021-01-14 11:41:21
tags: 
categories: DesignPatterns
---

#### 	静态代理和动态代理



​	静态代理 :  编译的时候就已经存在，

​	动态代理 ： 通过反射机制生成的代理对象

https://juejin.cn/post/6844903520919879694

https://juejin.cn/post/6844903978342301709

https://time.geekbang.org/column/article/201823



Kotlin 委托

```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b

fun main() {
    val b = BaseImpl(10)
    Derived(b).print()
}
```

