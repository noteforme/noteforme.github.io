---
title: hot_patch（热更新）
comments: true
date: 2017-10-17 15:25:56
tags:
categories: ANDROID
---

为了预防项目紧急问题，使用热更新比发版要好点

##### 热更新选择



* VitrualApk  滴滴开源的方案，之前的dynamic-load-apk推动的热更新的发展,不过这个开源没多久，准备后面迁移到这上面来

* Tinker-Bugly      这个很不错免费，就是只支持 冷启动，配合bugly教程用起来方便

* sophix         这个支持热启动 配置也方便，不过5万设备以上就要收费，对于收费我觉得也无可厚非，毕竟是别人的劳动成果，
                 作为开发者的角度，还是开源最好，以后可以学习下。

所以先选择Tinker-Bugly

##### 接入

 按照[接入文档](https://bugly.qq.com/docs/user-guide/instruction-manual-android-hotfix/ "bugly") 配合Demo接入配置，然后就开始做自己的app 定义

1. 修改appid 

 bugly平台申请后的 AppId

```
 // 这里实现SDK初始化，appId替换成你的在Bugly平台申请的appId
        // 调试时，将第三个参数改为true
        Bugly.init(getApplication(), "f7b6508f55", true);
```
 第三个参数设置  log是否展示

2. tinkerId

每次修改 基线版本和 补丁版本都要修改 tinkerId

```
 tinkerId = "1.0.6-patch"

```

3. 点击 assembleDebug

生成 基线版本

https://bugly.qq.com/docs/user-guide/instruction-manual-android-hotfix-demo/

```
实际应用中，请注意保存线上发布版本的基准apk包、mapping文件、R.txt文件，如果线上版本有bug，就可以借助我们tinker-support插件进行补丁包的生成。
```

4. 修改 def baseApkDir , 修改 tinkerId

* 修改bug
* baseApkDir是根据基线版本生成的目录
* 修改tinkerId

5. 编译patch包 

  点击 buildTinkerPatchRelease，生成patch_signed_7zip.apk上传,然后重启就可以了

6. 对于开发设备和全量设备

修改

```
//指定为开发设备
   Bugly.setIsDevelopmentDevice(getApplication(),true);
```

   

 参考 ：
  https://bugly.qq.com/docs/user-guide/instruction-manual-android-hotfix/
  https://github.com/Tencent/tinker
  https://github.com/BuglyDevTeam/Bugly-Android-Demo



##### UncaughtExceptionHandler Bugly冲突 不上报日志

https://blog.csdn.net/ZPCrobot/article/details/97390156



##### 从零开始实现一个插件化框架

https://blog.csdn.net/qq_22090073/article/details/103946596



https://www.jianshu.com/p/af8c47fabb12