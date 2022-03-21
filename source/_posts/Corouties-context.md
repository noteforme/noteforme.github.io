---
title: Corouties_context
date: 2022-03-20 17:48:54
tags: coroutie
categories: Kotlin
---



### 协程的上下文

##### 协程的上下文

![2022-03-20_5.51.40](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_5.51.40.png)

![2022-03-20_5.57.50](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_6.02.42.png)



##### 协程上下文的继承

![2022-03-20_7.03.14](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.03.14.png)



![2022-03-20_7.10.47](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.10.47.png)



![2022-03-20_7.31.59](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.31.59.png)

### 异常

##### 异常处理的必要性

![2022-03-20_7.33.31](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.33.31.png)



##### 异常的传播

![2022-03-20_7.34.53](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.34.53.png)

##### 43自动传播异常 与向用户暴露异常



![2022-03-20_7.39.07](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.39.07.png)



根协程GlobalScope 调用.wait才会抛出异常

##### 44 非根协程的异常传播

![2022-03-20_7.42.51](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.42.51.png)

##### 45异常的传播特性

![2022-03-20_7.43.54](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.43.54.png)

##### 46 SupervisorJob

![2022-03-20_7.45.55](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.45.55.png)



一个协程有问题 不会影响其他的协程

![2022-03-20_7.49.09](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.49.09.png)

Supervisor.cancel()



##### 47SupervisorScope



![2022-03-20_7.58.18](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.58.18.png)

![2022-03-20_7.59.57](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_7.59.57.png)

作用域抛出异常，导致子协程取消



### 异常的捕获

##### 异常的捕获

![2022-03-20_8.06.19](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.06.19.png)

![2022-03-20_8.09.06](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.09.06.png)

![2022-03-20_8.11.56](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.11.56.png)



![2022-03-20_8.13.11](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.13.11.png)

异常处理器不能安装到内部协程，要安装到外部协程



##### 50 异常捕获 防止App闪退

![2022-03-20_8.19.42](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.19.42.png)

##### 51Android全局异常处理

![2022-03-20_8.20.19](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.20.19.png)



![2022-03-20_8.22.33](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.22.33.png)

![2022-03-20_8.24.33](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.24.33.png)

![2022-03-20_8.25.08](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.25.08.png)

![2022-03-20_8.25.20](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.25.20.png)

##### 52取消与异常

![2022-03-20_8.26.57](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.26.57.png)







异常取消顺序， 先自己

![2022-03-20_8.29.30](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.29.30.png)



![2022-03-20_8.36.25](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.36.25.png)

![2022-03-20_8.37.57](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.37.57.png)



##### 异常聚合

suppressed 捕获多个异常

![2022-03-20_8.42.04](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.42.04.png)



![2022-03-20_8.41.02](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.41.02.png)

![2022-03-20_8.41.48](/Users/m/Documents/BLOG/source/_posts/Corouties-context/2022-03-20_8.41.48.png)



