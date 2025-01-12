---
title: AndroidSource
comments: true
date: 2018-07-25 09:57:29
tags: AOSP
categories:  ANDROID

---

Android docker

[史上最简单Android源码编译环境搭建方法 | Weishu's Notes](https://weishu.me/2016/12/30/simple-way-to-compile-android-source/)

https://source.android.com/docs/setup/start/requirements#setting-up-a-linux-build-environment

https://www.zhihu.com/people/tian-weishu/answers?page=1
https://zwc365.com/2020/08/30/android10-baiduwangpan

# ubuntu environment

ubuntu environment

## user permission

1. Open terminal.
2. Type "su root" in the terminal and press enter
3. You will be asked to enter the password. Type the password and press enter. You will be moved to root.
   4.Type "usermod -aG sudo username". Add your username, and enter. Nothing will happend. You will move to next line without any error.
4. Reboot/Restart the os.
   https://www.youtube.com/watch?v=ZxOwFOtcaaA comment

# download source code

https://mirrors.ustc.edu.cn/help/aosp.html

1. create bin

```
mkdir ~/bin
PATH=~/bin:$PATH
## curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
## 如果上述 URL 不可访问，可以用下面的：
curl -sSL  'https://gerrit-googlesource.proxy.ustclug.org/git-repo/+/master/repo?format=TEXT' |base64 -d > ~/bin/repo
chmod a+x ~/bin/repo
```

2. Dir

```
mkdir AOSP
cd AOSP
repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest
```

## 如果提示无法连接到 gerrit.googlesource.com，可以编辑 ~/bin/repo，把 REPO_URL 一行替换成下面的：

```
REPO_URL = 'https://gerrit-googlesource.proxy.ustclug.org/git-repo'
```

ubuntu 20.04 运行repo init 提示 /usr/bin/env: ‘python’: No such file or directory 解决方案

sudo ln -s /usr/bin/python3 /usr/bin/python

https://juejin.cn/post/7071152327482146823

同步源码树（以后只需执行这条命令来同步）：

repo sync

https://blog.csdn.net/qq_34508943/article/details/133391020

Install required packages
To install required packages for Ubuntu 18.04 or later, run the following command:

```
 sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```

https://source.android.com/docs/setup/start

# SOURCE CODE BUILD



[android 12 源码编译与虚拟机调试_aosp running multiple emulators with the same avd-CSDN博客](https://blog.csdn.net/qq_17696807/article/details/124302856)



1. 初始化编译环境

      source build/envsetup.sh

        

2. 选择产品  
   使用lunch选择要编译的产品，此文档中以编译x86_x64 emulator模拟器镜像为例进行说明。

修改AndroidProduct.mk使支持x86_x64镜像编译
由于android12 默认lunch默认选不到模拟器镜像，所以首先需要修改mk。
修改build/make/target/product/AndroidProducts.mk文件，添加sdk_phone_x86_64-eng支持

```
diff --git a/target/product/AndroidProducts.mk b/target/product/AndroidProducts.mk
index 7d9d90e92a..419cccb80a 100644
--- a/target/product/AndroidProducts.mk
+++ b/target/product/AndroidProducts.mk
@@ -84,3 +84,4 @@ COMMON_LUNCH_CHOICES := \
     aosp_arm-eng \
     aosp_x86_64-eng \
     aosp_x86-eng \
+    sdk_phone_x86_64-eng \

```



原文链接：https://blog.csdn.net/qq_17696807/article/details/124302856


lunch sdk_phone_x86_64-eng



## SWAP RAM

```
# Turn swap off
# This moves stuff in swap to the main memory and might take several minutes
sudo swapoff -a
# Create an empty swapfile
# Note that "1G" is basically just the unit and count is an integer.
# Together, they define the size. In this case 16GB.
sudo dd if=/dev/zero of=/swapfile bs=1G count=16
# Set the correct permissions
sudo chmod 0600 /swapfile
sudo mkswap /swapfile  # Set up a Linux swap area
sudo swapon /swapfile  # Turn the swap on


Check if it worked

grep Swap /proc/meminfo


Make it permanent (persist on restarts)
Add this line to the end of your /etc/fstab:

/swapfile swap swap sw 0 0
```

[Aosp 14 build error [100% 1/1] analyzing Android.bp files and generating ninja file at out/soong/build.ninja FAILED: out/soong/build.ninja - Stack Overflow](https://stackoverflow.com/questions/77278089/aosp-14-build-error-100-1-1-analyzing-android-bp-files-and-generating-ninja-f)

要学习Android源码需要编译一份，然后安装要求导入AndroidStudio,可以参考:
http://blog.csdn.net/huaiyiheyuan/article/details/52069122

## build issue



#### libncurses.so.5

```
  prebuilts/clang/host/linux-x86/clang-3289846/bin/clang.real: error while loading
 shared libraries: libncurses.so.5: cannot open shared object file: No such file
 or directory
```

[32 bit - How to install libncurses.so.5 in Ubuntu 20.04? - Ask Ubuntu](https://askubuntu.com/questions/1252062/how-to-install-libncurses-so-5-in-ubuntu-20-04)

https://blog.csdn.net/qq_34508943/article/details/133391020



- get into ***/etc/apt/sources.list.d*** and locate ***ubuntu.sources***.
- open the terminal in that directory by right clicking in it.
- then run ***sudo nano ./ubuntu.sources*** a pluma editor will open.
- just add these lines:-

```
Types: deb
URIs: http://security.ubuntu.com/ubuntu 
Suites: focal-security
Components: main universe
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

[AOSP 编译Android12源码全记录 - 简书](https://www.jianshu.com/p/53941de91c77)



#### python error

[/usr/bin/env: ‘python’: No such file or directory-CSDN博客](https://blog.csdn.net/weixin_55940238/article/details/140989311)



docker error



[Unable to compile AOSP source code on Ubuntu 24.04 system - Stack Overflow](https://stackoverflow.com/questions/78857564/unable-to-compile-aosp-source-code-on-ubuntu-24-04-system)

https://github.com/alsutton/aosp-build-docker-images

* Activity启动过程
  
  对应用程序Activity进行编译和打包
  
      /home/jon/桌面/LaoLuo/chapter-7/src/packages/experimental/Activity
      make snod
      emulator

然后查看activity信息，在这里通过源码里面的 adb

    cd  /home/jon/AOSP/out/host/linux-x86/bin
    adb shell dumpsys activity

<img src="https://img-blog.csdn.net/20170123173332254?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaXRhY2hpODU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" style="zoom: 67%;" />

##### Android open source project

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
>     https://blog.csdn.net/wen0006/article/details/5804639
> 
> ##### 源码关联阅读

也可以选择对应的文件的 .class文件后，再选择源码后再建立关联。

![20220527125047](AndroidSource/20220527125047.jpg)

https://www.jianshu.com/p/8012d5d38b01

[Ubuntu 24.04 + Windows 10/11 双引导系统无损安装 | AI开源项目 模型微调必备 - YouTube](https://www.youtube.com/watch?v=EXyuSOSMt4A)

https://www.zhihu.com/people/tian-weishu/answers?page=5

Binder

https://www.zhihu.com/question/39440766/answer/81511893

[Binder学习指南 | Weishu's Notes](https://weishu.me/2016/01/12/binder-index-for-newer/)



https://github.com/satur9nine/aosp-docker-build-env
