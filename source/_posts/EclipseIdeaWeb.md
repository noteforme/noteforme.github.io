---
title: EclipseIdeaWeb
comments: true
date: 2017-07-23 19:16:39
tags: 
categories: TOOL

---
##  Eclipse导入web项目

1、src并入包
导入项目后可以看这种情况

​    ![DS712](EclipseIdeaWeb\DS712.png)


处理方法：VideoServer右键－>Build Path->Configure Build Path->Source->Brower->src->Apply;接着就可以看到

![DS5817](EclipseDWeb\DS5817.png),点击Yes后，项目目录正常了，但是发现这是一个Ｊava项目,没有javax.相关的包

- ２、Java项目转Ｗeb项目
VideoServer右键－>Properties->Project Facets
- 在这里勾选　Dynamic Web Module:下面会有个提示　Ｄynamic Web Module 3.0 Requires Java 1.6 or newer.
- 勾选　Ｊava　　![藐三](EclipseDWeb/DS3156.png)
- 勾选　ＪavaScript
点击Apply

##  Idea安装
http://justcode.ikeepstudying.com/2017/12/%E6%9B%B4%E6%96%B0-intellij-idea-2017-3-1-%E6%B3%A8%E5%86%8C%E7%A0%81-%EF%BC%88%E4%BA%B2%E6%B5%8B%E5%8F%AF%E7%94%A8%EF%BC%89/

http://idea.lanyus.com/

https://www.jetbrains.com/help/idea/developing-a-java-ee-application.html




