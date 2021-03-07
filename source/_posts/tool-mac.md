---
title: tool_mac
comments: true
date: 2020-04-11 16:35:50
tags:
categories: TOOL
---



some tool for mac

查看当前环境变量 : env



Packet Sender  

tcp ip tool

<img src="tool-mac/Screen Shot 2020-04-11 at 4.40.59 PM.png" alt="Screen Shot 2020-04-11 at 4.40.59 PM" style="zoom:67%;" />



#### HomeBrew安装

1. 下载

https://raw.githubusercontent.com/Homebrew/install/master/install.sh

2. 更换源

   ```
   BREW_REPO="https://github.com/Homebrew/brew"
      换成  
   BREW_REPO="git://mirrors.ustc.edu.cn/brew.git" 
   ```

3. 权限

   ```
   chmod 755 install.sh 
   ./install.sh 
   ```

   

   https://blog.csdn.net/weixin_43635647/article/details/104249968

   

   ##### set environment variable mac

   1. Flutter

   ```
   vi ~/.bash_profile
   export PATH="$PATH:/Users/john/development/flutter/bin"
   
   which flutter
   
   ```

   2. jdk 

      https://docs.oracle.com/javase/10/install/installation-jdk-and-jre-macos.htm#JSJIG-GUID-F575EB4A-70D3-4AB4-A20E-DBE95171AB5F

   

   #####  mac terminal

   1. download and install iterm2
   2. Download iterm2-finder-tools-1.0.0  open Open iTerm.workflow install
   3. file top 右键 customize toolbar then move opem iTerm into toolbar

   

   

4. Ignore

   `Command + Shift + .`

5. Environment set

   AppledeMacBook-Pro:~ apple$ cat ~/.bash_profile

   export PATH="$PATH:/Users/john/development/flutter/bin"

   export PUB_HOSTED_URL=https://pub.flutter-io.cn

   export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn 



* gradle 阿里云镜像 

  /Users/john/.gradle 目录添加 init.gradle