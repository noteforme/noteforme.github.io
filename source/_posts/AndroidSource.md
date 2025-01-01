---
title: AndroidSource
comments: true
date: 2018-07-25 09:57:29
tags: AOSP
categories:  ANDROID

---

Android docker

[史上最简单Android源码编译环境搭建方法 | Weishu's Notes](https://weishu.me/2016/12/30/simple-way-to-compile-android-source/)

https://www.bilibili.com/video/BV15W411L7Lc?p=9

##### 源码调试

要学习Android源码需要编译一份，然后安装要求导入AndroidStudio,可以参考:
http://blog.csdn.net/huaiyiheyuan/article/details/52069122

```
public class Application extends ContextWrapper implements ComponentCallbacks2 {}
```

当我点开父类ContextWrapper后，发现引用的是jar立面的class文件，既然有源码这肯定不是我所需要的，
可以这要操作:Project Structure->Dependencies(可以看到很多jar依赖删掉)->　＋JARS And Dierctories ->添加源码要关联的frameworks 、packages...

1. 新建JDK1.8　选择openjdk8

　![](AndroidSource/DeepinScrot_aosp1.png)

2. 删除依赖
   
   　![](AndroidSource/DeepinScrot_aosp5.png)

3.　选择需要的包

![](AndroidSource/DeepinScrot_aosp.png)

4.　这一步要衡量一下，转为gradle项目后，project structure下面就没有moudles了
 ![](AndroidSource/DeepinScrot_aosp2.png)
    这个设置后麻烦可以大了，后面不得不删除了android.ipr、android.iws、android.iml这三个文件重新生成

５.　跳转到源码
　　找到pacages-> apps->Settings　`public class SettingsDrawerActivity extends Activity {`

  点开Activity发现还是跳转到jar的内容  

6. 最主要的是这一步,直接添加aosp最外层路径导入源码就可以了

<img src="AndroidSource/aosp6.png" alt="aosp6" style="zoom:50%;" />

  终于成功了

然后就可以愉快的调试源码了

/home/jon/noteforme.github.io/public/2017/08/10/DesignParrerns

https://cloud.tencent.com/developer/news/277549

* Activity启动过程
  
  对应用程序Activity进行编译和打包
  
      /home/jon/桌面/LaoLuo/chapter-7/src/packages/experimental/Activity
      make snod
      emulator

然后查看activity信息，在这里通过源码里面的 adb

    cd  /home/jon/AOSP/out/host/linux-x86/bin
    adb shell dumpsys activity

> 

<img src="https://img-blog.csdn.net/20170123173332254?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaXRhY2hpODU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" style="zoom: 67%;" />

1.Android系统架构
Android系统架构分为五层，从上到下依次是应用层、应用框架层、系统运行库层、硬件抽象层和Linux内核层。

应用层
系统内置的应用程序以及非系统级的应用程序都是属于应用层。负责与用户进行直接交互，通常都是用Java进行开发的。

应用框架层（Java Framework)
应用框架层为开发人员提供了可以开发应用程序所需要的API，我们平常开发应用程序都是调用的这一层所提供的API，当然也包括系统的应用。这一层的是由Java代码编写的，可以称为Java Framework。下面来看这一层所提供的主要的组件。

名称    功能描述
Activity Manager(活动管理器)    管理各个应用程序生命周期以及通常的导航回退功能
Location Manager(位置管理器)    提供地理位置以及定位功能服务
Package Manager(包管理器)    管理所有安装在Android系统中的应用程序
Notification Manager(通知管理器)    使得应用程序可以在状态栏中显示自定义的提示信息
Resource Manager（资源管理器）    提供应用程序使用的各种非代码资源，如本地化字符串、图片、布局文件、颜色文件等
Telephony Manager(电话管理器)    管理所有的移动设备功能
Window Manager（窗口管理器）    管理所有开启的窗口程序
Content Providers（内容提供器）    使得不同应用程序之间可以共享数据
View System（视图系统）    构建应用程序的基本组件
表1
系统运行库层（Native)
系统运行库层分为两部分，分别是C/C++程序库和Android运行时库。下面分别来介绍它们。

1.C/C++程序库
C/C++程序库能被Android系统中的不同组件所使用，并通过应用程序框架为开发者提供服务，主要的C/C++程序库如下表2所示。

名称    功能描述
OpenGL ES    3D绘图函数库
Libc    从BSD继承来的标准C系统函数库，专门为基于嵌入式Linux的设备定制
Media Framework    多媒体库，支持多种常用的音频、视频格式录制和回放。
SQLite    轻型的关系型数据库引擎
SGL    底层的2D图形渲染引擎
SSL    安全套接层，是为网络通信提供安全及数据完整性的一种安全协议
FreeType    可移植的字体引擎，它提供统一的接口来访问多种字体格式文件
表2
2.Android运行时库
运行时库又分为核心库和ART(5.0系统之后，Dalvik虚拟机被ART取代)。核心库提供了Java语言核心库的大多数功能，这样开发者可以使用Java语言来编写Android应用。相较于JVM，Dalvik虚拟机是专门为移动设备定制的，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik 应用作为一个独立的Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。而替代Dalvik虚拟机的ART 的机制与Dalvik 不同。在Dalvik下，应用每次运行的时候，字节码都需要通过即时编译器转换为机器码，这会拖慢应用的运行效率，而在ART 环境中，应用在第一次安装的时候，字节码就会预先编译成机器码，使其成为真正的本地应用。

硬件抽象层（HAL)
硬件抽象层是位于操作系统内核与硬件电路之间的接口层，其目的在于将硬件抽象化，为了保护硬件厂商的知识产权，它隐藏了特定平台的硬件接口细节，为操作系统提供虚拟硬件平台，使其具有硬件无关性，可在多种平台上进行移植。 从软硬件测试的角度来看，软硬件的测试工作都可分别基于硬件抽象层来完成，使得软硬件测试工作的并行进行成为可能。通俗来讲，就是将控制硬件的动作放在硬件抽象层中。

Linux内核层
Android 的核心系统服务基于Linux 内核，在此基础上添加了部分Android专用的驱动。系统的安全性、内存管理、进程管理、网络协议栈和驱动模型等都依赖于该内核。
Android系统的五层架构就讲到这，了解以上的知识对以后分析系统源码有很大的帮助。

2.Android系统源码目录
我们要先了解Android系统源码目录，为后期源码学习打下基础。关于源码的阅读，你可以访问http://androidxref.com/来阅读系统源码。当然，最好是将源码下载下来。下载源码可以使用清华大学开源软件镜像站提供的Android 镜像：https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/ 。如果觉得麻烦也可以查找国内的网盘进行下载，推荐使用该百度网盘地址下载：http://pan.baidu.com/s/1ngsZs，它提供了多个Android版本的的源码下载。

整体结构
各个版本的源码目录基本是类似，如果是编译后的源码目录会多增加一个out文件夹，用来存储编译产生的文件。Android7.0的根目录结构说明如下表所示。

从表3可以看出，系统源码分类清晰，并且内容庞大且复杂。接下来分析packages中的内容，也就是应用层部分。
应用层部分
应用层位于整个Android系统的最上层，开发者开发的应用程序以及系统内置的应用程序都是在应用层。源码根目录中的packages目录对应着系统应用层。它的目录结构如表4所示。

##### Android open source project

##### 

Android Architecture

<img src="AndroidSource/android_stack_720.png" style="zoom:50%;" />         <img src="AndroidSource/ape_fwk_all.png" style="zoom: 67%;" />

https://source.android.com/                            

 https://source.android.com/devices/architecture

>         Android源码根目录    描述
>     
>     abi    应用程序二进制接口
>     art    全新的ART运行环境
>     bionic    系统C库
>     bootable    启动引导相关代码
>     build    存放系统编译规则及generic等基础开发包配置
>     cts    Android兼容性测试套件标准
>     dalvik art    虚拟机
>     developers    开发者目录
>     development    应用程序开发相关
>     device    设备相关配置
>     docs    参考文档目录
>     external    开源模组相关文件
>     frameworks    应用程序框架，Android系统核心部分，由Java和C++编写
>     hardware    主要是硬件抽象层的代码
>     libcore    核心库相关文件
>     libnativehelper    动态库，实现JNI库的基础
>     ndk    NDK相关代码，帮助开发人员在应用程序中嵌入C/C++代码
>     out    编译完成后代码输出在此目录
>     packages    应用程序包
>     pdk    Plug Development Kit 的缩写，本地开发套件
>     platform_testing    平台测试
>     prebuilts    x86和arm架构下预编译的一些资源
>     sdk    sdk和模拟器
>     system    底层文件系统库、应用和组件
>     toolchain    工具链文件
>     tools    工具文件
>     Makefile    全局Makefile文件，用来定义编译规则
>     ————————————————
>     
>     https://blog.csdn.net/wenzhi20102321/article/details/80739649
>     
>     https://blog.csdn.net/wen0006/article/details/5804639

##### 源码关联阅读

也可以选择对应的文件的 .class文件后，再选择源码后再建立关联。

![20220527125047](AndroidSource/20220527125047.jpg)

https://www.jianshu.com/p/8012d5d38b01



[Ubuntu 24.04 + Windows 10/11 双引导系统无损安装 | AI开源项目 模型微调必备 - YouTube](https://www.youtube.com/watch?v=EXyuSOSMt4A)
