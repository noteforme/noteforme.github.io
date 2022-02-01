---
title: AMS
comments: true
date: 2020-08-07 22:04:13
tags: AOSP
categories: 
---



#### 系统启动

![](AMS/2021-07-31 at 7.53_start1.png)

​	![](AMS/2021-08-16_PHONE_START.png)



<img src="AMS/2021-07-31 at 7.53.43_start2.png" style="zoom: 50%;" />







#### AMS

AMS属于SystemServer进程,主要是为了加载Activity 管理Activity生命周期 .

<img src="AMS/2021-08-07_8.25_ams.png" style="zoom:50%;" />

Launcher请求AMS创建根Activity所在进程（如果之前没有该进程），AMS请求Zygote进程fork应用进程。





![](AMS/2021-08-16_AMS_APPLICATION.png)



![](AMS/2021-08-07_8.26_ams.png)





android启动流程

https://www.bilibili.com/video/BV1DQ4y197oX?from=search&seid=14802265503756746151

https://www.bilibili.com/video/BV1Pi4y1A7Sr?from=search&seid=17031756056912732811





一般 https://www.bilibili.com/video/BV1K541177GU?p=3 



#### 应用启动流程

##### Launcher请求AMS阶段

![](AMS/2021-08-16_lancher_ams.png)
