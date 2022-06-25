---
title: coroutie-flow
date: 2022-03-20 20:46:54
tags: coroutie
categories: Kotlin
---



### 56Flow 异步流

![2022-03-20_8.47.50](coroutie-flow/2022-03-20_8.47.50.png)



##### 异步返回多个值的方案

集合 , 序列 , 挂起函数 ,Flow



##### 如何表示多个值

![2022-03-20_8.49.02](coroutie-flow/2022-03-20_8.49.02.png)

![2022-03-20_8.58.42](coroutie-flow/2022-03-20_8.58.42.png)

![2022-03-20_9.00.07](coroutie-flow/2022-03-20_9.00.07.png)

##### 通过Flow异步返回多个值

  

**可以去掉suspend**

![2022-03-20_9.02.24](coroutie-flow/2022-03-20_9.02.24.png)

![2022-03-20_9.03.12](coroutie-flow/2022-03-20_9.03.12.png)

再起一个任务，确认线程是否阻塞

![2022-03-20_9.04.46](coroutie-flow/2022-03-20_9.04.46.png)



##### Flow与其他方式的区别

![2022-03-20_9.06.40](coroutie-flow/2022-03-20_9.06.40.png)

##### Flow应用

![2022-03-20_9.09.34](coroutie-flow/2022-03-20_9.09.34.png)

### 流的特性

##### 60 冷流

![2022-03-20_9.12.12](coroutie-flow/2022-03-20_9.12.12.png)

类似于懒加载



61.流的连续性



![2022-03-20_10.09.29](coroutie-flow/2022-03-20_10.09.29.png)

62. asFlow 

    flowOf

    

63. 流上下文

    flowOn

64. 知道协程中收集流

    launchIn()

65. 流的取消

    withTimeoutOrNull

66. 流的取消检测

    Cancellable()

67. 背压

    buffer

    

101.  stateFlow

     ![20220619220530](coroutie-flow/20220619220530.jpg)

102. sharedFlow

     
