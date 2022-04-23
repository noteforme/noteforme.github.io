---
title: coroutine_begin
comments: true
date: 2021-11-08 21:18:20
tags: coroutie
categories: coroutine
---





![xieceng](coroutine-begin/2021-11-08_9_15_13.png)







![2021-11-08_9.20.36](coroutine-begin/2021-11-08_9.20.36.png)

![2021-11-08_9.20.36](coroutine-begin/2021-11-08_9.34.44.png)



挂起阻塞的区别![2021-12-13_9.23.31_](coroutine-begin/2021-12-13_9.23.31_.png)

![2021-12-13_9.41.35_](coroutine-begin/2021-12-13_9.53.43_.png)

![2021-12-13_9.57.42_](coroutine-begin/2021-12-13_9.57.42_.png)

![2021-12-13_9.59.02_](coroutine-begin/2021-12-13_10.09.47_.png)





![2021-12-13_10.09.47_](coroutine-begin/2021-12-13_10.09.47_.png)

![2021-12-13_10.18.50_](coroutine-begin/2021-12-13_10.18.50_.png)





### cpu密集型任务取消

##### 29cpu密集型任务取消 isActive

<img src="coroutie-start/2022-03-20_11.31.18.png" alt="2022-03-20_11.31.18" style="zoom:67%;" />

退出循环



![2022-03-20_12.17.26](coroutie-start/2022-03-20_12.17.26.png)



##### yiled取消

<img src="coroutie-start/2022-03-20_12.18.27.png" alt="2022-03-20_12.18.27" style="zoom:67%;" />

<img src="coroutie-start/2022-03-20_12.19.29.png" alt="2022-03-20_12.19.29" style="zoom:50%;" />



##### coroutine取消副作用

<img src="coroutie-start/2022-03-20_12.26.32.png" alt="2022-03-20_12.26.32" style="zoom:67%;" />

<img src="coroutie-start/2022-03-20_12.27.16.png" alt="2022-03-20_12.27.16" style="zoom:67%;" />

##### 标准函数use

自动关闭文件对象

![2022-03-20_12.29.38](coroutie-start/2022-03-20_12.29.38.png)

![2022-03-20_12.30.36](coroutie-start/2022-03-20_12.30.36.png)



##### 34不能取消的任务

![2022-03-20_12.32.18](coroutie-start/2022-03-20_12.32.18.png)

![2022-03-20_12.34.23](coroutie-start/2022-03-20_12.34.23.png)



##### 35超时任务

网络请求超时

![2022-03-20_12.38.56](coroutie-start/2022-03-20_12.38.56.png)

