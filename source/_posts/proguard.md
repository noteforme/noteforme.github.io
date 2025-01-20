---
title: proguard
comments: true
date: 2017-12-19 07:29:23
tags: proguard
categories: ANDROID
---

#  ProGuard中间文件 
开启ProGuard，Android工程被Build后，会生成以下文件：<project_root>\app\build\outputs\mapping\release 

- dump.txt 
  apk文件中所有类的构成一览 
- mapping.txt 
  记录了混淆后的名字与混淆前的名字的对应关系，每一次混淆结果和映射关系都不一样。 
  当遇到Bug是，查看到的堆信息，要和混淆前的源码关联起来，所以管理这个文件很重要。 
  retrace.bat -verbose mapping.txt stacktrace.txt 
>com.rensanning.example.androidsample.User -> com.rensanning.example.androidsample.g: 
>   java.lang.String name -> a 
>   java.lang.String hometown -> b 
>   java.util.ArrayList getUsers() -> a

- seeds.txt  
  未被混淆的类和方法一览 

- usage.txt 
  记录了从apk文件中删掉的代码。这个文件一定要认真确认，是否这些代码真的是多余的。



https://developer.android.com/build/shrink-code



https://www.jianshu.com/p/6a07046e65f4

http://blog.csdn.net/guolin_blog/article/details/50451259


https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA

##### apktool 判断是否混淆

* 最快的方式

  把Apk后缀改成 zip ,解压后把 classes.dex 文件拖到 jadx里面

https://blog.csdn.net/xiaohai695943820/article/details/72367896

https://www.itread01.com/content/1548717866.html

* 反编译工具介绍

https://www.cnblogs.com/zhen-android/p/7830249.html





**脱糖** 即在编译阶段将在语法层面一些底层字节码不支持的特性转换为基础的字节码结构，(比如List上的泛型脱糖后在字节码层面实际为Object)； Android工具链对Java8语法特性脱糖的过程可谓丰富多彩，当然他们的最终目的是一致的：使更新的语法可以在所有的设备上运行。


https://juejin.cn/post/6844903828756643854

