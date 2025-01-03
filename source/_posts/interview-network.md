---
id: 231
title: 'INTERVIEW NETWORK'
date: '2024-05-27T06:33:00+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=231'
permalink: /2024/05/27/interview-network/
categories:
    - INTERVIEW
    - NETWORK
---

- 1.ARP协议:在IP以太网中，当一个上层协议要发包时，有了该节点的IP地址，ARP就能提供该节点的MAC地址。
- 2.HTTP HTTPS的区别: 
  - 1.HTTPS使用TLS(SSL)进行加密
  - 2.HTTPS缺省工作在TCP协议443端口
  - 3.它的工作流程一般如以下方式:
  - 1.完成TCP三次同步握手
  - 2.客户端验证服务器数字证书，通过，进入步骤3
  - 3.DH算法协商对称加密算法的密钥、hash算法的密钥
  - 4.SSL安全加密隧道协商完成
  - 5.网页以加密的方式传输，用协商的对称加密算法和密钥加密，保证数据机密性；用协商的hash算法进行数据完整性保护，保证数据不被篡改
  - 3.http请求包结构，http返回码的分类，400和500的区别
  - 1.包结构： 
      - 1.请求：请求行、头部、数据
      - 2.返回：状态行、头部、数据
  - 2.http返回码分类：1到5分别是，消息、成功、重定向、客户端错误、服务端错误
  - 4.Tcp
  - 1.可靠连接，三次握手，四次挥手 
      - 1.三次握手：防止了服务器端的一直等待而浪费资源，例如只是两次握手，如果s确认之后c就掉线了，那么s就会浪费资源
      - 1.syn-c = x，表示这消息是x序号
      - 2.ack-s = x + 1，表示syn-c这个消息接收成功。syn-s = y，表示这消息是y序号。
      - 3.ack-c = y + 1，表示syn-s这条消息接收成功
  - 2.四次挥手：TCP是全双工模式 
      - 1.fin-c = x , 表示现在需要关闭c到s了。ack-c = y,表示上一条s的消息已经接收完毕
      - 2.ack-s = x + 1，表示需要关闭的fin-c消息已经接收到了，同意关闭
      - 3.fin-s = y + 1，表示s已经准备好关闭了，就等c的最后一条命令
      - 4.ack-c = y + 1，表示c已经关闭，让s也关闭
  - 3.滑动窗口，停止等待、后退N、选择重传
  - 4.拥塞控制，慢启动、拥塞避免、加速递减、快重传快恢复