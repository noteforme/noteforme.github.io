---
title: JCenterPublish
comments: true
date: 2019-12-13 06:30:57
tags: 
categories: TOOL
---





##### nexus sonatype 

1. 主要步骤按照这个操作 https://www.gcssloop.com/gebug/maven-private

2. 注意： 密码不再是　admin admin 123了按照这个步骤 https://www.jianshu.com/p/fcb128e34c87

3. 部署好一个平台后，sonatype-work复制过去，其他配置就不用再弄了，注意nexus-3.20.0-04-unix.tar.gz解压后也会有sonatype-work文件，可能会把之前的覆盖

4. 运行

   在命令行工具中输入启动命令：

   software/nexus-3.20.0-04-mac/nexus-3.20.0-04/bin

   ```
   ./nexus start
   ```

   如果一切顺利，在等待几十秒到一两分钟之后就可以查看我们的仓库了，如果出错了，可以使用 run 命令来查看具体的出错原因：

   ```
   # run 命令相当于 debug 模式，会输出所有的日志信息
   ./nexus run
   ```

   

   当然，Nexus 还有很多其他命令(例如:停止、重启、查看状态等)：

   ```
   ./nexus {start|stop|run|run-redirect|status|restart|force-reload}
   ```

   注意:windows启动是这样的 `.\nexus.exe /run`

5. 打包

 在配置完善后同步一下项目，就可以打开 gradle 命令菜单看到多出来了3个命令，双击即可执行对应的命令：

- pack：打包项目
- uploadToLocal：上传到本机仓库
- uploadToPublic：上传到公网仓库

6. 查看

   `http://localhost:8081` 即可查看，如果修改了端口号，后面写对应的端口号即可。如果是运行在服务器上，则在其他电脑上输入`http://{服务器ip}:{port}` IP 和对应的端口号。如果运行成功，则会看到类似如下界面：





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



