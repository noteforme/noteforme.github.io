---
title: tool_mac
comments: true
date: 2020-04-11 16:35:50
tags:
categories: TOOL
---

##### JDK环境变量

##### adb 环境变量

```
sudo touch ~/.zshrc
sudo vi ~/.zshrc

复制

export ANDROID_HOME=/Users/m/Library/Android/sdk
export PATH=$ANDROID_HOME/platform-tools:$PATH
export PATH=$ANDROID_HOME/tools:$PATH
export PATH=$ANDROID_HOME/tools/bin:$PATH
```

https://dev.to/ravics09/solution-of-command-not-found-adb-error-29e7

[Installation of the JDK and the JRE on macOS](https://docs.oracle.com/javase/10/install/installation-jdk-and-jre-macos.htm#JSJIG-GUID-F575EB4A-70D3-4AB4-A20E-DBE95171AB5F)

Mac系统的环境变量，加载顺序为：  
/etc/profile /etc/paths ~/.bash_profile ~/.bash_login ~/.profile ~/.bashrc

##### IntelliJ idea clion

clion激活: CLion-2019.2.5.dmg 下载完成后,先使用再打开,jetbrains-agent-20200227.zip直接拖入就好了。

some tool for mac

查看当前环境变量 : env

##### Packet Sender

tcp ip tool

<img src="tool-mac/Screen Shot 2020-04-11 at 4.40.59 PM.png" alt="Screen Shot 2020-04-11 at 4.40.59 PM" style="zoom:67%;" />

##### HomeBrew安装

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

##### mac terminal

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

##### gradle 阿里云镜像

/Users/john/.gradle 目录添加 init.gradle

##### 鼠须管默认简繁体

control 加上 ~ 快捷键

##### 屏幕录制

https://blog.csdn.net/qq_43428139/article/details/109038098

https://www.cnblogs.com/hi3254014978/p/15084851.html#:~:text=%E8%A7%A3%E5%86%B3Mac%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%94%B5%E8%84%91%E8%87%AA%E5%B8%A6%E5%BD%95%E5%B1%8F%E8%BD%AF%E4%B



https://www.youtube.com/watch?v=KjL_sJS9Rko&t=430s

https://www.youtube.com/watch?v=LSmM5FXzVBg&t=320s