---
title: AndroidStudioTool
comments: true
date: 2017-08-13 18:20:17
tags:
categories: "工具"
---

# 快捷键

>  1.Ctrl＋E，可以显示最近编辑的文件列表
2.Shift＋Click可以关闭文件
3.Ctrl＋[或]可以跳到大括号的开头结尾
4.Ctrl＋Shift＋Backspace可以跳转到上次编辑的地方
5.Ctrl＋F12，可以显示当前文件的结构
6.Ctrl＋F7可以查询当前元素在当前文件中的引用，然后按F3可以选择
7.Ctrl＋N，可以快速打开类
8.Ctrl＋Shift＋N，可以快速打开文件
9.Alt＋Q可以看到当前方法的声明
10.Ctrl＋W可以选择单词继而语句继而行继而函数
11.Alt＋F1可以将正在编辑的元素在各个面板中定位
12.Ctrl＋P，可以显示参数信息
13.Ctrl＋Shift＋Insert可以选择剪贴板内容并插入
14.Alt＋Insert可以生成构造器/Getter/Setter等
15.Ctrl＋Alt＋V 可以引入变量。例如把括号内的SQL赋成一个变量
16.Ctrl＋Alt＋T可以把代码包在一块内，例如try/catch
17.Alt＋Up 和 Alt＋Down可在方法间快速移动
18.Ctrl+ H 查看类的继承关系


参考：https://github.com/1sters/Android-Studio-Guide/blob/master/tips-shortcuts.md

# 降低　compileSdkVersion　版本
 　有时候需要看低版本的源码，就要修改compileSdkVersion版本

1. 修改编译版本
   ![图片](AndroidStudioTool/DeepinScrot_compile.png)
2. targetSdkVersion版本修改成　21   然后   compile 'com.android.support:appcompat-v7:21.+' 继承的AppCompatActivity改成Activity

3. 编译后报错　Error:(11) No resource identifier found for attribute 'roundIcon' in package 'android'
  根据错误删除　android:roundIcon ,然后编译


  参考：http://blog.csdn.net/hyr83960944/article/details/39941683



# android studio gradle编译问题
   
  编译也根据不同情况做不同处理


## 导入项目

修改Gradle配置文件后就一直卡在那，在　build.gradle值修改下面

如果用了ss代理，在ubuntu设置http没用，可以在gradle-wrapper.properties 添加

`
org.gradle.jvmargs=-DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080
`

参考: https://www.zhihu.com/question/37810416


平常github下载项目导入AndroidStudio直接卡死，心里真不是...., 目前实验一种方式应该会快点
修改 gradle\wrapper\gradle-wrapper.properties下的 distributionUrl=https\://services.gradle.org/distributions/gradle-3.3-all.zip， 我的AndroidStudio默认是这个 所以就修改成这样的.


## 下载gradle

  有时候想用新的的gradle，但是新的更新几十M的文件，一天都不一定能下下来
   直接去官网下载 https://gradle.org/releases/  ，对应版本的zip文件，放到相应目录
  比如我的就是 C:\Users\Administrator\.gradle\wrapper\dists\gradle-3.3-all\55gk2rcmfc6p2dg9u9ohc3hw9\



## 修改Log颜色

android studio log默认都是白色的，在 setting -> Android Log下去掉Use inherited attributes

我的颜色按照这个修改的
http://www.jianshu.com/p/e3f8f7383c3d

