---
title: ubuntu_guide
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

## 解压乱码

在windows上压缩的文件，是以系统默认编码中文来压缩文件。由于zip文件中没有声明其编码，所以linux上的unzip一般以默认编码解压，中文文件名会出现乱码。
可以使用终端zip文件进行解压并指定解压的编码
　`unzip -O GBK   xxx.zip`
解压后的文件的路径为当前终端所在的路径

参考：　[Eric Chan](http://hareric.com/2016/07/26/%E7%AE%80%E6%98%93%E6%96%B9%E6%B3%95%E8%A7%A3%E5%86%B3ubuntu%E4%B8%8B%E8%A7%A3%E5%8E%8B%E6%96%87%E4%BB%B6%E4%B9%B1%E7%A0%81/) 


** 17.10的问题　**

*  Android studio3.1无法输入中文
  　使用 fcitx 选择sunpingying  也可以选择 google pinyin
    　然后输入这两行命令
		
			pidof fcitx | xargs kill  #找到原有的fcitx端口并kill掉
			fcitx                     #开启fcitx

参考： 　https://blog.csdn.net/he729164860/article/details/78484713
  　
  　
  　

## 安装Gnome　shell

```
		sudo apt-get install gnome-tweak-tool
		sudo apt-get install chrome-gnome-shell
```
打开tweak可以看到按钮

浏览点击 ||| off按钮系统就会安装

开源chromium加载有问题，使用firefox

很有用的插件

https://extensions.gnome.org/extension/545/hide-top-bar/

参考:https://www.mobibrw.com/2017/9342

　

##  ctrl + alt + left （Intellij idea shortcut conflict）

* install <u>Dconf Editor</u>   in ubuntu software

* launch <u>Dconf Editor</u>, go to    /org/gnome/desktop/wm/keybindings/switch-to-workspace-left  

  double click like below

  ![df](ubuntu_guide/ubuntu_guide_01.png)

 use default value(click Off) -> Custom value(like me or else)

* you could use it in intellij idea now .

####  genymotion安装

* 下载Virtualbox
* 下载genymotion
* 下载Android stuido genymotion插件

```
chmod +x genymotion-2.6.0-linux_x64.bin      
./genymotion-2.6.0-linux_x64.bin -d /home/qiu/Work/genymotion  
```

//注意： chmod +x 有空格

#### 输入法设置

ubuntu 18.04 intellij 输入法选择有问题，可以用命令行安装 ibus-pinyin，然后input souce选择拼音输入法