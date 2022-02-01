---
title: Package-signature
comments: true
date: 2017-09-21 10:56:13
tags:
categories: ANDROID

---

####  Apk打包流程

<img src="Package-signature/2021-08-01_3.apk_package.png" style="zoom:50%;" />







## 相同签名 不同包名问题

现在把项目重新生成签名，然后安装，安装后会安装失败，因为和以前的app冲突

应用安装后会在data/data 目录生成和包名一样的文件夹

虽然生成了了新的签名但是会覆盖

## 打包

android studio更新了新版本 打包的时候v1和v2都需要勾选才能安装

http://blog.h5min.cn/daihuimaozideren/article/details/77842549


## walle打包

生成渠道包

* 生成渠道包 ./gradlew clean assembleReleaseChannels 
* 支持 productFlavors ./gradlew clean 
    assembleMeituanReleaseChannels


https://github.com/Meituan-Dianping/walle

