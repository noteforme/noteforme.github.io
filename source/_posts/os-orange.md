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

先安装

https://www.shuzhiduo.com/A/lk5a1VAN51/

环境变量

再安装下面的

```bash
sudo apt-get install g++

sudo apt-get install build-essential

sudo apt-get install libgtk2.0-dev

sudo apt-get install bison

sudo apt-get install libx11-dev libxrandr-dev libsdl1.2-dev vgabios

sudo apt-get install xorg-dev 
```

bochs -version就可以了

接下来按照这个文档跑

[Orange‘s:一个操作系统的实现学习笔记1_orange's一个操作系统的实现-CSDN博客](https://blog.csdn.net/weixin_42193791/article/details/123144920)

[bochs安装配置_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Hj421f77x?spm_id_from=333.788.videopod.sections&vd_source=d4c5260002405798a57476b318eccac9)

上面video也是按照这个步骤

https://blog.csdn.net/lhl_blog/article/details/76785193?app_version=6.2.9&code=app_1562916241&csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%2276785193%22%2C%22source%22%3A%22m0_62832651%22%7D&uLinkId=usr1mkqgl919blen&utm_source=app

## virturlbox

enable the **Shared Clipboard** and install the **VirtualBox Guest Additions**. [[1](https://dev.to/bala_audu_musa/copy-paste-not-working-between-windows-and-ubuntu-virtualbox-heres-the-fix-pa3), [2](https://askubuntu.com/questions/73059/how-to-copy-paste-from-ubuntu-virtualbox-guest-to-windows-host)]

Follow these steps to get it set up:

1. Enable Bidirectional Clipboard

2. Open VirtualBox and make sure your Ubuntu Virtual Machine (VM) is powered off.

3. Click on the VM and select **Settings**.

4. Go to the **General** tab, then click on the **Advanced** tab.

5. Set **Shared Clipboard** and **Drag'n'Drop** to **Bidirectional**.

Install VirtualBox Guest Additions

- Start your Ubuntu virtual machine.
- Once booted, go to the top menu bar of the VirtualBox window and click **Devices** > **Insert Guest Additions CD Image**.
- A prompt may appear asking you to run the software. If so, click **Run** and enter your password.
- If a CD icon appears on your Ubuntu desktop, right-click it and select **Open in Terminal**.
- Inside the terminal, run the installer by typing:  
  `sudo sh ./VBoxLinuxAdditions.run`
- Enter your password and press **Enter**.
- Once the installation is complete, restart your Ubuntu VM. [[1](https://unix.stackexchange.com/questions/569993/copy-paste-not-working-on-virtual-box-6-1-running-ubuntu-18-04-on-windows-10-mac), [2](https://www.youtube.com/watch?v=q5rdX-2LJXg), [3](https://askubuntu.com/questions/992264/copying-text-from-terminal), [4](https://dev.to/bala_audu_musa/copy-paste-not-working-between-windows-and-ubuntu-virtualbox-heres-the-fix-pa3), [5](https://www.youtube.com/watch?v=xGB9jt-8OUs)]



# share folder

```
 sudo adduser $USER vboxsf
```

https://www.youtube.com/watch?v=h_adwDledYM



安装bochs



```bash
tar vxzf bochs-2.6.9.tar.gz

cd bochs-2.6.9

./configure --enable-debugger --with-sdl --enable-disasm  --enable-readline LIBS='-lX11'
make
sudo make install
```







运行

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


//ubuntu

sudo mkdir /home/m/Documents/floppy/
sudo mount -o loop pm.img /home/m/Documents/floppy/
sudo cp pmtest2.com /home/m/Documents/floppy/
sudo umount /home/m/Documents/floppy


 sudo mkdir /mntfloppy
 nasm boot.asm -o boot.bin
 nasm loader.asm -o loader.bin
 dd if=boot.bin of=a.img bs=512 count=1 conv=notrunc 
 sudo mount -o loop a.img /mntfloppy/
 sudo cp loader.bin /mntfloppy/
 sudo umount /mntfloppy/


bochs
c
//运行模拟器后 到bochx 虚拟机，B磁盘下
b:
//运行
pmtest2.com
```

# 五内核

```bash
 ld -m elf_i386 -s -o hello  hello.o
```

加载kernel

```bash
 nasm boot.asm -o boot.bin
 nasm loader.asm -o loader.bin
 nasm -f elf kernel.asm -o kernel.o
 ld -m elf_i386 -s  kernel.o -o kernel.bin 
 dd if=boot.bin of=a.img bs=512 count=1 conv=notrunc 
 sudo mkdir /mntfloppy
 sudo mount -o loop a.img /mntfloppy/
 sudo cp loader.bin /mntfloppy/
 sudo cp kernel.bin /mntfloppy/
 sudo umount /mntfloppy/
```
