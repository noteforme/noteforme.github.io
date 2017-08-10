---
title: AndroidSource
comments: true
date: 2017-07-25 09:57:29
tags:
categories: AOSP

---

要学习Android源码需要编译一份，然后安装要求导入AndroidStudio,可以参考:
http://blog.csdn.net/huaiyiheyuan/article/details/52069122

```
public class Application extends ContextWrapper implements ComponentCallbacks2 {}
```
当我点开父类ContextWrapper后，发现引用的是jar立面的class文件，既然有源码这肯定不是我所需要的，
可以这要操作:Project Structure->Dependencies(可以看到很多jar依赖删掉)->　＋JARS And Dierctories ->添加源码要关联的frameworks 、packages...





Activity启动过程
 对应用程序Activity进行编译和打包
 
      /home/jon/桌面/LaoLuo/chapter-7/src/packages/experimental/Activity
      make snod
      emulator
      
然后查看activity信息，在这里通过源码里面的 adb

    cd  /home/jon/AOSP/out/host/linux-x86/bin
    adb shell dumpsys activity
    


/home/jon/noteforme.github.io/public/2017/08/10/DesignParrerns