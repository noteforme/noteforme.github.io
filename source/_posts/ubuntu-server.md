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

3. `rz`命令上传，会弹出一个框选择文件
   
   查看图片 http://45.77.222.97/homedetail_pic.png

##### JDK Environment ubuntu

1. when I wget tar.gz from oracle office.  I couldn't tar tar.gz ,so I upload one.
   
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

# SSH  Server

```
~/.ssh/known_hosts
```

**Install SSH server:**

```bash
sudo apt update
sudo apt install openssh-server
```

**Start and enable the SSH service:**

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

connect with another teminal

```bash
ssh username@ip_address
```

ssh远程连接Ubuntu（局域网和非局域网）

内网穿透

[ssh远程连接Ubuntu（局域网和非局域网）_ssh连接ubuntu-CSDN博客](https://blog.csdn.net/qq_43566431/article/details/129823135) 2.非局域网 远程连接
Attention:ssh只能用于可以相互ping通的主机之间

使用Zerotier可以将接入的设备放在一个虚拟的局域网中，这样你在外网，只要机子配置了Zerotier和另一台机子在一个Network中，就可以直接访问了，废话不多说看操作。

1.登录zerotier 官网 创建一个new network ，复制NETWORK ID

上图中的NETWORK ID就是命令行中的你的network ID
2.Ubuntu安装zerotier

curl -s [https://install.zerotier.com](https://install.zerotier.com) | sudo bash
1
3.Ubuntu加入zerotier局域网

sudo zerotier-cli join 你的network ID
1
4.Windows安装zerotier并加入局域网
客户端下载地址：[Download - ZeroTier](https://www.zerotier.com/download/) 安装之后右键join network，输入你的network ID

5.返回zerotier 官网在Ubuntu和Windows前面打勾 授权

6.在Windows客户端打开ssh连接，输入上图中Ubuntu的Manager IPs，即可连接成功。

3. Zerotier常用命令：
   
   启动Zerotier
   
   sudo systemctl start zerotier-one.service
   
   重启Zerotier
   
   sudo systemctl restart zerotier-one.service
   
   设置开机自启动
   
   sudo systemctl enable zerotier-one.service
   
   查看服务状态
   
   zerotier-cli status

        加入网络

zerotier-cli join 8850xxxxxxxxxxxxxxx

     离开网络

zerotier-cli leave 8850xxxxxxxxxxxxxxx

查看所有网络

zerotier-cli listnetworks
————————————————

[ssh远程连接Ubuntu（局域网和非局域网）_ssh连接ubuntu-CSDN博客](https://blog.csdn.net/qq_43566431/article/details/129823135)

# Visit desktop

8:00 端口转发

13:20 DDNS-Go

[外网访问家庭内网的两大最优方案，零基础教程 远程控制家庭电脑 - YouTube](https://www.youtube.com/watch?v=t3H8kVQmTu0)

[如何从外网远程桌面访问自己的电脑 - YouTube](https://www.youtube.com/watch?v=NZVxl5-efSs&t=316s)

[Ubuntu技巧-Ubuntu远程访问之电信公网IP-CSDN博客](https://blog.csdn.net/koffuxu/article/details/141399290)

[https://www.youtube.com/watch?v=NZVxl5-efSs![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main//tools2025-01-11%20-11.54.26.png)](https://www.youtube.com/watch?v=NZVxl5-efSs!%5B%5D(https://raw.githubusercontent.com/BlogForMe/ImageServer/main//tools2025-01-11%20-11.54.26.png))

[外网访问家庭内网的两大最优方案，零基础教程 远程控制家庭电脑 ，公网访问家庭局域网 - 哔哩哔哩](https://www.bilibili.com/opus/931018269756751908)

[Ubuntu技巧-Ubuntu远程访问之电信公网IP-CSDN博客](https://blog.csdn.net/koffuxu/article/details/141399290)

route config 端口转发

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/TOOLS/2025-01-1111.54.26.png)



试用xrdp 远程访问

一直访问不了，重启后又可以了，估计是要设置密码，ubuntu电脑不能先进入到桌面.

```
sudo apt update
sudo apt install xrdp

sudo systemctl enable xrdp
sudo ufw allow from any to any port 3389  proto tcp
```

[Linux科学上网工具，支持Clash和v2ray，linux翻墙clash客户端，虚拟机翻墙 &#8211; 科技分享](https://www.kjfx.cc/524.html#google_vignette)
