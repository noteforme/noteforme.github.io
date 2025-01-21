---
title: AOSP_LOG
date: 2025-01-12 11:12:40
tags: AOSP
---

[安卓巴士Android系统定制十八-修改系统APP后进行编译-科技-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTcxMzkxNg==.htm)

[安卓巴士Android系统定制十九-Dalvik&amp;ART的区别及ODEX文件介绍-教育-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTcxNjA3Mg==.html)

[安卓巴士Android系统定制二十-编译时odex化的原因-教育-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTcyOTI4OA==.html)

[安卓巴士Android系统定制二十一-修改Calclator.apk代码并运行-教育-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTczNzEyOA==.html)

[安卓巴士Android系统定制二十二-Framework定制及Mac环境介绍-教育-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTczODE2OA==.html)

[安卓巴士Android系统定制二十五-liblog.so(native层）的修改与编译-科技-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTc2MTM3Ng==.html)

time: 8:00

```
source build/envsetup.sh

make systemimage


mm -B            // clean firstly then build 


/home/m/ANDROID12/out/target/product/generic/system/app/
```

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/AOSP/Screenshot%202025-01-12%20at%2011.39.18.png)

# FILE Permission

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/AOSP/Screenshot%202025-01-12%20at%2013.05.07.png)

[安卓巴士android系统定制四常见Linux命令介绍-下-教育-高清完整正版视频在线观看-优酷](https://v.youku.com/v_show/id_XMzAxOTcwNDYwNA==.html)

# start flow 启动流程

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/AOSP/60dca538fa6041fd8e6d38606cc7b1bd%7Etplv-k3u1fbpfcp-zoom-in-crop-mark_1512_0_0_0.webp)

[AOSP下的系统开发第一个方向：以前是搞android应用开发，现在负责系统application开发，这样就会上手非常 - 掘金](https://juejin.cn/post/6872647317393145864)

在packages/apps找到需要修改的相应app。在该app目录中是属于java的部分，与android应用的开发语言相同。但是与平常的app在**androidstudio**编写不同，在源码环境中，需要利用**编辑器**来进行编辑。并且在源码中的编译就需要利用make编译系统进行编译。源码的编译是以模块进行。利用mm命令进行编译。编译完成之后，会生成**odex以及apk**。与普通的apk的结构不同，**系统编译生成的application分成了两部分：**

1. apk包括了签名，资源，AndroidMannifest清单文件,资源的二进制文件。
2. 而原本的classes.dex则被放在了odex这一部分中。

### 为什么apk分成了两个文件，这就是Dalvik与art虚拟机的区别：

**Dalvik**：  
JIT(just in time)**实时编译**,运行的时候将字节码翻译成机器码,所运行的目标文件与硬件平台无关,app运行效率低  
**ART**:
AOT(Ahead of time)**预先编译**,运行前将字节码翻译成机器码,所运行的目标文件(oat)与硬件平台相关.app运行效率高。但会占用空间。APK安装所需时间增加

### 2.1 odex是干什么的

- **Dalvik时代**：apk**运行**的时候，会把apk的classes.dex解压出来并通过**dexopt**优化为.odex文件,缓存在/data/dalvik-cache目录下,提高后续执行的效率。这也是**android被诟病的比较卡**。
- **ART时代**：apk**安装**的时候，会把apk的classes.dex解压后,通过dex2oat工具转化为.odex文件(ELF格式，与Dalvik时代的.odex文件完全不同，只是为了和以前的Dalvik时代的命名相同，内容完全不同),**存储在apk所在目录的oat目录下**。**用空间换时间**

#### 2.2 为什么在源码环境编译生成了odex文件

ROM：apk，jar，bin，so等组成  
优点:

1. 降低系统更新后启动的时间：  
   未做odex的Rom，首次开机的过程会执行odex操作。编译时做，开机时候就不用做了
2. 减少在设备上进行odex操作所造成的空间浪费：  
   编译时进行dexopt/dex2oat，会直接将apk资源与代码拆开。如果在设备上安装时进行dexopt/dex2oat,apk的大小不会减少,但又会多一个odex文件占据磁盘空间

缺点:

1. 增加开发的编译时间
2. 不能直接执行apk的install操作,需要将apk和odex都sync到设备上

#### 2.2.1 如何在开发阶段关闭dex2oat

- 在当前module的Android.mk里增加: `LOCAL_DEX_PREOAT = false`
- 在build/core/main.mk中关闭所有module的dex优化:

链接：https://juejin.cn/post/6872647317393145864  



# Application change

### change App Calender

AllInOneActivity

```java
    @Override
    protected void onCreate(Bundle icicle) {
        super.onCreate(icicle);
        Log.wtf(TAG, "jon app developer");
    }
```



### replace app

```
adb install -r '/home/m/ANDROID12/out/target/product/emulator_x86_64/product/app/Calendar/Calendar.apk' 

```

### check logs

```
adb shell
emulator_x86_64:/ # logcat -s "AllInOneActivity"
--------- beginning of main
01-21 21:22:29.426  1914  1914 E AllInOneActivity: jon app develope
```



Framework change
