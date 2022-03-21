---
title: AndroidStudioTool
comments: true
date: 2017-08-13 18:20:17
tags:
categories: TOOL
---



#### 降低　compileSdkVersion　版本
 　有时候需要看低版本的源码，就要修改compileSdkVersion版本

1. 修改编译版本
   ![图片](AndroidStudioTool/DeepinScrot_compile.png)
2. targetSdkVersion版本修改成　21   然后   compile 'com.android.support:appcompat-v7:21.+' 继承的AppCompatActivity改成Activity

3. 编译后报错　Error:(11) No resource identifier found for attribute 'roundIcon' in package 'android'
    根据错误删除　android:roundIcon ,然后编译

  参考：http://blog.csdn.net/hyr83960944/article/details/39941683


#### 　android stuido代理

##### 普通代理
修改Gradle配置文件后就一直卡在那，在　build.gradle值修改下面
如果用了ss代理，在ubuntu设置http没用，可以在 工程根目录下 gradle.properties  添加

`org.gradle.jvmargs=-DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080
`
参考: https://www.zhihu.com/question/37810416

平常github下载项目导入AndroidStudio直接卡死，心里真不是...., 目前实验一种方式应该会快点
修改 gradle\wrapper\gradle-wrapper.properties下的 distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip， 我的AndroidStudio默认是这个 所以就修改成这样的.

#####  jcenter库代理　(这几句话花了关键得一天时间)

前天升级了系统，之前下得Demo一直跑不起来，'https://jitpack.io'得文件一直下载不下来　
　＞错误：　jcenter.bintray.com:443 failed to respond
　折腾了一天终于弄好了,需要设置proxy代理,把socket流量转为http
　然后在　Ｍanual proxy configuration下面选择HTTP 填上
　 ` 127.0.0.1 　 8118
　 `
　 就可以下载jitpack里面得文件了



##### 阿里云镜像

> /Users/john/.gradle/init.gradle




##### 下载gradle

  有时候想用新的的gradle，但是新的更新几十M的文件，一天都不一定能下下来
   直接去官网下载 https://gradle.org/releases/  ，对应版本的zip文件，放到相应目录
  比如我的就是 C:\Users\Administrator\.gradle\wrapper\dists\gradle-3.3-all\55gk2rcmfc6p2dg9u9ohc3hw9\

还一个是在${home}/.gradle目录下得gradle.properties文件配置应该也是可以的




#### 快捷键

>Ctrl＋F12，可以显示当前文件的结构
> Ctrl＋Shift＋F7 可以高亮当前元素在当前文件中的使用 
> Ctrl＋E，可以显示最近编辑的文件列表
>  Alt＋Up 和 Alt＋Down可在方法间快速移动
>  Shift＋Click可以关闭文件
>  Ctrl+ H 查看类的继承关系
>  Ctrl＋Alt＋B可以跳转到抽象方法的实现
>  Ctrl＋Q可以看JavaDoc



##### 全局修改快捷键

`Shift+F6`

想要将里面的`cardName`全部替换成`userName`，则选中其中一个，按`shift+f6`回车即可批量修改。

参考：https://github.com/1sters/Android-Studio-Guide/blob/master/tips-shortcuts.md

常用快捷键:https://mp.weixin.qq.com/s/lYBHtg342-t3NkPPY9E-YQ



##### debug问题

> 15:40	Error running 'app'
> 		Cannot debug application from module app on device huawei-pra_al00-HMKNW17A12007001.
> 		This application does not have the debuggable attribute enabled in its manifest.
> 		If you have manually set it in the manifest, then remove it and let the IDE automatically assign it.
> 		If you are using Gradle, make sure that your current variant is debuggable.



Build Variants 设置成debug

```groovy
  debug {
          debuggable false
          minifyEnabled false
          signingConfig signingConfigs.debug
      }
```



debuggable设置成 true



#####  SVN分支合并 主干,主干新增文件被删除

弄了两个多小时，说多了都是泪

https://www.jianshu.com/p/e50af339259f



##### android命名规范

https://juejin.im/entry/5b6a4ca9f265da0f4c6fe566

##### 无线调试

<https://juejin.im/entry/5a6a7e69518825733b0f1635>

设置侦听断开 :

```
adb tcpip 8888

adb connect 192.168.31.76:8888
```

安装包安装

`adb -s acac34d7 install /e/JYWORK/`

##### win10 jdk环境变量设置

在系统变量中设置 ,注意要点开编辑文本是不是之前输入的有 ""



##### Android stuido发布项目Jcenter

```
gradlew clean build bintrayUpload  -PbintrayUser=blogforme  -PbintrayKey=4c7511bba437157d77baeb5c17d339ce92c2bee7  -PdryRun=false

```

https://github.com/novoda/bintray-release

https://www.jianshu.com/p/9f81d5b5a451

https://blog.csdn.net/qq_32452623/article/details/79282605



Apk瘦身

https://blog.csdn.net/qq_32175491/article/details/80071987

也有不建议删SO库的说法

http://kaedea.com/2016/06/04/android-dynamical-loading-04-so-problems/





##### 依赖其他moudle

https://mp.weixin.qq.com/s/trAxRzz573TFyJk2klKdag



##### android stuido 日志

##### 过滤不需要的日志

```
^(?!.*(eglMakeCurrent|OpenGLRenderer)).*$

eglMakeCurrent OpenGLRenderer两个包含需要过滤的字段
```

##### 留下需要的日志

<img src="AndroidStudioTool/Screen Shot 2021-02-06 at 11.57.50 AM.png" style="zoom: 80%;" />

https://www.jianshu.com/p/11e56991ff28