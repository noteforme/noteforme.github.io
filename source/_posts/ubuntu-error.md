---
title: ubuntu-error
comments: true
date: 2017-09-20 21:42:17
tags:
categories:  LINUX
---
　**解决Ubuntu 16.04 系统的一些错误**

##  Unable to mount root fs on unknown-block
　看到Ubuntu系统有更新就点了下，早上打开电脑 Ubuntu直接进不去最下面弹出这这样的
　错误　Unable to mount root fs on unknown-block　而且只能强制关机，心想完了我的
　资料，还有辛苦编译的源码，不过好在晚上回来倒腾了一把　终于好了。

步骤
1、  Advanced options for Ubuntu 
２、ubuntu Linux 4.4.0-96... (recovery mdoe)  　这里我选择的是第二个，第一个点击后还是
　　会报错
3、Menu界面　，把能清的先清了，然后点击grub,执行后续操作

4、登录进入命令行界面

```
sudo mount -o rw //这一行和参考资料不同
sudo dpkg --configure -a　//在这里等了很久 Device ....Start的字样,接着就按了　Ctrl + Alt + Delete , 重启就好了

sudo apt-get install -f  # to finish upgrades 最好加这个吧
type reboot
```

  https://askubuntu.com/questions/875862/kernel-panic-unable-to-mount-root-in-ubuntu-16-04/875868


　
　
　
　