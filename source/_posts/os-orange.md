---
title: os-orange
date: 2026-02-24 22:25:42
tags:
categories:  OS
---

# orange环境

代码仓库

https://gitee.com/jiang000/oranges

## ubuntu

```bash
sudo apt-get install build-essential 
sudo apt-get install xorg-dev 
sudo apt-get install bison 
sudo apt-get install g++ 
./configure --enable-debugger --enable-disasm
make
sudo make install
```

接下来按照这个文档跑

[Orange‘s:一个操作系统的实现学习笔记1_orange's一个操作系统的实现-CSDN博客](https://blog.csdn.net/weixin_42193791/article/details/123144920)

[bochs安装配置_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Hj421f77x?spm_id_from=333.788.videopod.sections&vd_source=d4c5260002405798a57476b318eccac9)

上面video也是按照这个步骤

https://blog.csdn.net/lhl_blog/article/details/76785193?app_version=6.2.9&code=app_1562916241&csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%2276785193%22%2C%22source%22%3A%22m0_62832651%22%7D&uLinkId=usr1mkqgl919blen&utm_source=app

#### 运行

 mac也是

```bash
nasm boot.asm -o boot.bin
dd if=boot.bin of=a.img  bs=512 count=1

//输入bochs
bochs
c
```

此时bochs已经开始运行了，但是还是黑屏，还需要在终端输入c，此时软盘里还没有东西，所以会提示一个选项，点击continue ，才会开始启动模拟，如下图所示：

修改

```bash
xxd a.img > a.txt  

//创建软盘,再执行上面两行命令
bximage 
1
fd
```

[organe&#39;s一个操作系统实现实验一实验步骤_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1zN4y1n7QB?spm_id_from=333.788.videopod.sections&vd_source=d4c5260002405798a57476b318eccac9)

```bash
这个也需要修改
# filename of ROM images
romimage: file=/usr/local/share/bochs/BIOS-bochs-latest
vgaromimage: file=/usr/local/share/bochs/VGABIOS-lgpl-latest

# enable key mapping, using US layout as default.
for ubuntu
keyboard:  keymap=/usr/local/share/bochs/keymaps/x11-pc-us.map
for macos
keyboard: keymap=/usr/local/share/bochs/keymaps/sdl2-pc-us.map
```

## macos

```bash
./configure --prefix=/usr/local \
--enable-smp \
--enable-cpu-level=6 \
--enable-disasm \
--enable-x86-64 \
--enable-vmx=2 \
--enable-svm \
--enable-all-optimizations \
--enable-debugger \
--enable-debugger-gui \
--with-sdl2
```

If SDL2 fails, try:
--with-sdl

Build

make

(This may take several minutes.)

Install

`sudo make install`

error

### 2) Find the line around ~194

You’ll see something like:

`char *devname; ... if ((devname = strrchr(devpath, '/')) != NULL) {`

### 3) Change `devname` to `const char *`

Edit it to:

`const char *devname; ... if ((devname = strrchr(devpath, '/')) != NULL) {`

# 保护模式

实验a运行

需要代码自带的 a.img,因为 pmtest1.asm 没有55aa,自己创建的找不到盘

```bash
nasm pmtest1.asm -o pmtest1.bin
dd if=pmtest1.bin of=a.img bs=512 count=1 conv=notrunc
```

实验b

```bash
nasm pmtest2.asm -o pmtest2.com

//macos
sudo mkdir /Users/m/Documents/floppy/
sudo mount -o loop pm.img /Users/m/Documents/floppy/
sudo cp pmtest2.com /Users/m/Documents/floppy/
sudo umount /Users/m/Documents/floppy/


sudo mkdir /home/j/Documents/floppy/
sudo mount -o loop pm.img /home/j/Documents/floppy/
sudo cp pmtest2.com /home/j/Documents/floppy/
sudo umount /home/j/Documents/floppy



bochs
c
//运行模拟器后 到bochx 虚拟机，B磁盘下
b:
//运行
pmtest2.com
```





第四章



```bash

dd if=/dev/zero of=a.img bs=512  count=2880
dd if=boot.bin of=a.img bs=512 count=1 conv=notrunc

dd if=loader.bin of=a.img bs=512 seek=1 conv=notrunc
```
