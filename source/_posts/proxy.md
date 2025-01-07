---
id: 28
title: Proxy
date: '2024-05-12T03:52:15+00:00'
author: Jon
layout: post
guid: 'http://jonblog.site/?p=28'
permalink: /2024/05/12/proxy/
categories:
    - TOOL
---

# V2rayU 需要全局的才起作用

[https://pagespr.pages.dev/trojan]

[https://workerpr.huaiyiheyuan.workers.dev/trojan123]

[https://pagespr.pages.dev/trojan]

client needs fragments 分片feature

ws 端口(port)：7个http端口可任意选择(80、8080、8880、2052、2082、2086、2095)

tls 端口(port)：6个https端口可任意选择(443、8443、2053、2083、2087、2096)

# ssh远程连接Ubuntu（局域网和非局域网）

https://blog.csdn.net/qq_43566431/article/details/129823135
2.非局域网 远程连接
Attention:ssh只能用于可以相互ping通的主机之间

使用Zerotier可以将接入的设备放在一个虚拟的局域网中，这样你在外网，只要机子配置了Zerotier和另一台机子在一个Network中，就可以直接访问了，废话不多说看操作。

1.登录zerotier 官网 创建一个new network ，复制NETWORK ID

上图中的NETWORK ID就是命令行中的你的network ID
2.Ubuntu安装zerotier

curl -s https://install.zerotier.com | sudo bash
1
3.Ubuntu加入zerotier局域网

sudo zerotier-cli join 你的network ID
1
4.Windows安装zerotier并加入局域网
客户端下载地址：https://www.zerotier.com/download/
安装之后右键join network，输入你的network ID

5.返回zerotier 官网在Ubuntu和Windows前面打勾 授权

6.在Windows客户端打开ssh连接，输入上图中Ubuntu的Manager IPs，即可连接成功。

3. Zerotier常用命令：
   
   # 启动Zerotier
   
   sudo systemctl start zerotier-one.service
   
   # 重启Zerotier
   
   sudo systemctl restart zerotier-one.service
   
   # 设置开机自启动
   
   sudo systemctl enable zerotier-one.service
   
   # 查看服务状态
   
   zerotier-cli status

# 加入网络

zerotier-cli join 8850xxxxxxxxxxxxxxx

# 离开网络

zerotier-cli leave 8850xxxxxxxxxxxxxxx

# 查看所有网络

zerotier-cli listnetworks
————————————————

reference：https://blog.csdn.net/qq_43566431/article/details/129823135