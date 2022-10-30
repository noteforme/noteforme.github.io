---
title: coroutine_begin1
comments: true
date: 2021-11-08 21:18:20
tags: coroutie
---



##### 协程是什么 03



<img src="coroutine-begin1/2021-11-08_9_15_13.png" style="zoom:67%;" />





##### 协程的挂起与恢复 08

<img src="coroutine-begin1/2021-11-08_9.20.36.png" style="zoom:67%;" />

<img src="coroutine-begin1/2021-11-08_9.34.44.png" style="zoom:67%;" />



##### 挂起阻塞的区别 09

<img src="coroutine-begin1/2021-12-13_9.23.31_.png" style="zoom:67%;" />

![2021-12-13_9.41.35_](coroutine-begin1/2021-12-13_9.53.43_.png)



##### 协程的调度器 P13

1. Dispatchers.Main

2. Dispatchers.IO

3. Dispatchers.Default

   

![](coroutine-begin1/2021-12-13_9.57.42_.png)

#####  任务泄漏 P14

![20220727203824](coroutine-begin1/20220727203824.jpg)



##### 结构化并发 p15

使用结构化并发可以做到

![](coroutine-begin1/20220727204523.jpg)

##### CoutineScope P16

GlobalScope

![](coroutine-begin1/2021-12-13_10.09.47_.png)

mainscope协程 可以通过抛出异常取消.



![](coroutine-begin1/2021-12-13_10.09.47_.png)

![2021-12-13_10.18.50_](coroutine-begin1/2021-12-13_10.18.50_.png)



