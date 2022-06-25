---
title: JVM
comments: true
date: 2020-04-18 22:19:42
tags: JVM
categories: JAVA
---



[JVM方法区-第六章](https://noteforme.github.io/2020/01/04/JVM-METHOD/)

<img src="JVM/2020-04-16_8.22.33.png" alt="2020-04-16_8.22.33" style="zoom:50%;" />



![2020-04-16_8.29.39](JVM/2020-04-16_8.29.39.png)

![2020-04-16_8.41.39](JVM/2020-04-16_8.41.39.png)



![2020-04-16_8.43.41](JVM/2020-04-16_8.43.41.png)



https://www.iteye.com/blog/rednaxelafx-656951

https://www.bilibili.com/video/av75247289?p=2





这种情况全局变量 b: B是否会造成内存泄漏

```kotlin
class A {
    val b = B.getInstance()
}

class B {
    companion object {
        val b = B()
        fun getInstance(): B {
            return b
        }
    }
}
```

我觉得不会,B 并不指向A



https://www.bilibili.com/video/BV1GA4y197VR?spm_id_from=333.337.search-card.all.click&vd_source=d4c5260002405798a57476b318eccac9



mOpenEnv  加