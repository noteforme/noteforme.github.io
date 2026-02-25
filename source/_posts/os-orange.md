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

```bash
nasm boot.asm -o boot.bin
dd if=boot.bin of=a.img  bs=512 count=1


xxd a.img > a.txt  

//创建软盘,再执行上面两行命令
bximage 
1
fd
```

[organe&#39;s一个操作系统实现实验一实验步骤_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1zN4y1n7QB?spm_id_from=333.788.videopod.sections&vd_source=d4c5260002405798a57476b318eccac9)

## macos





```
for ubuntu
keyboard:  keymap=/usr/local/share/bochs/keymaps/x11-pc-us.map
for macos
keyboard: keymap=/usr/local/share/bochs/keymaps/sdl2-pc-us.map
```





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

sudo mkdir /mnt/floppy
sudo mount -o loop pm.img /mnt/floppy
sudo cp pmtest2.com /mnt/floppy
sudo umount /mnt/floppy


sudo mkdir /Users/m/floppy
sudo mount -o loop pm.img /Users/m/floppy
sudo cp pmtest2.com /Users/m/floppy
sudo umount /Users/m/floppy

```
