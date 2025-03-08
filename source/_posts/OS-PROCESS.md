---
title: OS_PROCESS
comments: true
date: 2020-08-17 10:55:14
tags:
categories: OS
---

#### 进程

进程有很大的独立性

为了实现进程模型，操作系统维护着一张表格，进程表。每个进程占用一个进程表项。

#### 进程通信

##### 进程通信方式

    文件
    AIDL （基于 Binder）
        Android 进阶：进程通信之 AIDL 的使用
        Android 进阶：进程通信之 AIDL 解析
    Binder
        Android 进阶：进程通信之 Binder 机制浅析
    Messenger （基于 Binder）
        Android 进阶：进程通信之 Messenger 使用与解析 
    ContentProvider （基于 Binder）
        Android 进阶：进程通信之 ContentProvider 内容提供者 
    Socket
        Android 进阶：进程通信之 Socket （顺便回顾 TCP UDP）

![](https://img-blog.csdn.net/20170605011532312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
原文链接：https://blog.csdn.net/u011240877/article/details/72863432

![](OS-PROCESS/2021-08-17_COMM_TYPE.png)

管道通信

无名管道， 有名管道:文件系统中有这样一个文件名

信号通信： 

##### 进程通信的思想

![](OS-PROCESS/2021-08-17_INTER_THINK.png)

**共享内存**

 共享存储允许不相关的进程访问同一片物理内存

共享内存是两个进程之间共享和传递数据最快的方式

共享内存未提供同步机制，需要借助其他机制管理访问

共享存储是两个进程之间共享和传递数据最快的一种方式

共享内存未提供同步机制，需要借助其他机制管理访问

**Unix域套接字**

域套接字是一种高级的进程间通信的方法

Unix域套接字可以用于同一机器进程间通信

https://www.bilibili.com/video/BV1tJ41117ty?from=search&seid=13006387505737298314
