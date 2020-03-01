---
title: supporting-devices
comments: true
date: 2017-09-23 21:41:20
tags:
categories:  ANDROID
---
Android屏幕的一些技巧

##### 官网适配方案

https://developer.android.com/training/basics/supporting-devices/index.html
https://developer.android.com/training/multiscreen/screensizes.html

##### 资源图片适配

**Table 1.** Configuration qualifiers for different pixel densities.

| Density qualifier | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| `ldpi`            | Resources for low-density (*ldpi*) screens (~120dpi).        |
| `mdpi`            | Resources for medium-density (*mdpi*) screens (~160dpi). (This is the baseline density.) |
| `hdpi`            | Resources for high-density (*hdpi*) screens (~240dpi).       |
| `xhdpi`           | Resources for extra-high-density (*xhdpi*) screens (~320dpi). |
| `xxhdpi`          | Resources for extra-extra-high-density (*xxhdpi*) screens (~480dpi). |
| `xxxhdpi`         | Resources for extra-extra-extra-high-density (*xxxhdpi*) uses (~640dpi). |
| `nodpi`           | Resources for all densities. These are density-independent resources. The system does not scale resources tagged with this qualifier, regardless of the current screen's density. |
| `tvdpi`           | Resources for screens somewhere between mdpi and hdpi; approximately 213dpi. This is not considered a "primary" density group. It is mostly intended for televisions and most apps shouldn't need it—providing mdpi and hdpi resources is sufficient for most apps and the system will scale them as appropriate. If you find it necessary to provide tvdpi resources, you should size them at a factor of 1.33*mdpi. For example, a 100px x 100px image for mdpi screens should be 133px x 133px for tvdpi. |


This means that if you generate a 200x200 image for xhdpi devices, you should generate the same resource in 150x150 for hdpi, 100x100 for mdpi, and 75x75 for ldpi devices.
而UI问我们需要多少大小的图，如果效果图根据750*1334的话，可以告诉他，不过PS有个叫
cutterman的工具可以自动生成不同分辨率的图片

- PX转 dp

以5X 为例: 5X的dpi =420,   PPI = 420/160 = 2.625
设置view的宽度我们用　match_parent,那么在5X上，他是多少dp呢？

5X : width = 1080 px
dp = 1080 / PPI = 411.428571429,
所以411dp就是5X的match_parent

http://blog.csdn.net/u010983881/article/details/51993157

##### 实际应用

* 开发中的实际转化

　　生成主流屏幕的values，安宽分成320份,高400份
　　
　　假设ＵＩ根据 iphone6(750×1334)设计的效果图
　　根据下图公式计算　Dpi

　   iphone6是4.7寸的 分辨率 750 * 1334,　它的dpi 就是　√(750²+1334²)/4.7 =  325 

​       和drawable-xhdpi 320接近
　　
　　drawable-xhdpi　480上做的图就是大概是1.5倍的样子 ,分辨率在1920 *1080上的也是一样的

　　http://blog.csdn.net/zengd0/article/details/52464627
　　所以说　给的1334 * 750 　图就是
　　10px  我们写5dp

​    参考:　http://www.jianshu.com/p/ec5a1a30694b
　http://blog.csdn.net/lmj623565791/article/details/45460089
　

* DPI参考

自己计算按照上面的公式很简单，最棘手的还是手机尺寸给出一个手机dpi网站 get what you want 
http://dpi.lv/

2.75625

https://mp.weixin.qq.com/s/v_aauFjx-f91WrpCAaNMVQ)

https://developer.android.com/guide/practices/screens_support

今日头条适配 

https://www.jianshu.com/p/55e0fca23b4f

https://github.com/JessYanCoding/AndroidAutoSize