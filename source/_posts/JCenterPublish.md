---
title: JCenterPublish
comments: true
date: 2019-12-13 06:30:57
tags: 
categories: TOOL
---





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



https://android.jlelse.eu/publishing-your-android-kotlin-or-java-library-to-jcenter-from-android-studio-1b24977fe450

##### maven center

按照这个教程参考**[TestMavenUp](git@121.41.17.19:library/TestMavenUp.git)*  就差不多了

https://www.jianshu.com/p/6c1d2688ed2d/

https://www.jianshu.com/p/0629548ab5a4



可以把项目和jar放到同一个目录



