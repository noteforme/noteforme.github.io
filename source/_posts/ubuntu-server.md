---
title: ubuntu_server
comments: true
date: 2017-11-20 21:11:58
tags:
categories:  TOOL
---



##### Linux下上传

http://www.jianshu.com/p/c3e4f96ced97
在html目录下放文件就可以了

`scp /home/jon/桌面/img.jpg  root@45.77.222.97:/var/www/html/`

* nexus-3.20.0-04: not a regular file

`scp -r /home/jon/桌面/img.jpg  root@45.77.222.97:/var/www/html/`



windows下上传

1. 安装Xshell

2. 给ubuntu安装管理器 `apt-get install -y lrzsz`

https://www.linuxidc.com/Linux/2015-05/117975.ht、

3.  `rz`命令上传，会弹出一个框选择文件

  查看图片 http://45.77.222.97/homedetail_pic.png

##### JDK Environment ubuntu

1.  when I wget tar.gz from oracle office.  I couldn't tar tar.gz ,so I upload one.

   > scp jdk-11.0.5_linux-x64_bin.tar.gz root@xxx/opt/java/
   >
   > tar -zxvf FileName.tar.gz

2. Edit  /etc/profile,  touch it if not have, 或者直接在根目录‘ cd / ’输入下面的这些，也可以，具体写入到哪先不管了

   > #set java JDK
   >
   > export JAVA_HOME=/opt/java/jdk1.8.0_231
   >
   > export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   >
   > export PATH=$JAVA_HOME/bin:$PATH

   

3. make it work

   > source /etc/profile

##### set jdk. on Mac

> ```
> Open ~/.bash_profile 
> export JAVA_HOME=$(/usr/libexec/java_home)
> source ~/.bash_profile
> ```



* 在64位机器上跑x86的jdk一直提示

> -bash: /opt/jdk/jdk1.8.0_231/bin/java: No such file or directory

free -m



