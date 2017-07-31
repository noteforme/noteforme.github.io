---
title: AndroidSource
comments: true
date: 2017-07-25 09:57:29
tags:
categories: AOSP

---
 Activity启动过程
 对应用程序Activity进行编译和打包
 
      /home/jon/桌面/LaoLuo/chapter-7/src/packages/experimental/Activity
      make snod
      emulator
      
然后查看activity信息，在这里通过源码里面的 adb

    cd  /home/jon/AOSP/out/host/linux-x86/bin
    adb shell dumpsys activity