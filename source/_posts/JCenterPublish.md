---
title: JCenterPublish
comments: true
date: 2019-12-13 06:30:57
tags: 
categories: TOOL
---

https://android.jlelse.eu/publishing-your-android-kotlin-or-java-library-to-jcenter-from-android-studio-1b24977fe450



##### nexus sonatype 

*  主要步骤按照这个操作 https://www.gcssloop.com/gebug/maven-private
* 注意： 密码不再是　admin admin 123了按照这个步骤 https://www.jianshu.com/p/fcb128e34c87

* 部署好一个平台后，sonatype-work复制过去，其他配置就不用再弄了，注意nexus-3.20.0-04-unix.tar.gz解压后也会有sonatype-work文件，可能会把之前的覆盖

 

##### Prepare

* Setting the JDK path

> ```
> Open ~/.bash_profile 
> export JAVA_HOME=$(/usr/libexec/java_home)
> source ~/.bash_profile
> ```