---
id: 196
title: 'Computer Network'
date: '2024-05-24T07:56:10+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=196'
permalink: /2024/05/24/computer-network/
categories:
    - NETWORK
---



Summary of 4 Layer Model
![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/20240524155816.jpg)

The 7-layer OSI Model
![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/20240524154936.jpg)

traceroute -w 1 www.jonblog.site

https://www.bilibili.com/video/BV1F54y1t7Dx?from=search&seid=12798261982184143495

# Open System Interconnection

OSI 7层模型

TCP/IP 5层模型

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_9.37_arp_router.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/network1.png)

# 应用层

## protocol

URI

> ftp://ftp.is.co.za/rfc/rfc1808.txt http://www.ietf.org/rfc/rfc2396.txt
> 
> ldap://[2001:db8::7]/c=GB?objectClass?one mailto:John.Doe@example.com
> 
> news:comp.infosystems.www.servers.unix
> 
> tel:+1-816-555-1212
> 
> telnet://192.0.2.16:80/ urn:oasis:names:specification:docbook:dtd:xml:4.1.2

## Http1.1

https://tools.ietf.org/html/rfc7230#section-6

HyperText Transfer Protocol 可靠的数据传输协议，基于TCP

### 报文格式

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-07-21_3.21_http_protical.png)

### Http2.0

https://tools.ietf.org/html/rfc7540

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-07-25_5.00_http2.png)

可以看到http2.0改造了协议， head 改成了 HEADERS frame ,Body 改成了 DATA frame。

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-07-25_5.04_http2.png)

- Length :  整个frame  开始到结束 
- Type :  frame的类型
- Stream ID用作流控制
- Payload 请求正文

https://www.bilibili.com/video/BV1SJ411q7ei?from=search&seid=10178848750670062838

https://www.bilibili.com/video/BV1Sw411d7oE?from=search&seid=10178848750670062838

# 端口作用

进程id是会变化的，进程通过绑定固定不变的端口，来实现通信。

http版本区别

https://juejin.cn/post/6844903489596833800

## Socket

对TCP/IP的封装， 提供可供程序员做网络开发的接口-Socket编程接口。

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-07-21_3.09_UML_HTTP.png)

SocketHttpTest.java

- http get请求
  
  按照get请求拼接字符串
  
  ```java
     StringBuffer protocol = new StringBuffer();
                  //请求行
                  protocol.append("GET ");
                  protocol.append(url.getPath());
                  protocol.append("?");
                  protocol.append(url.getQuery());
                  protocol.append(" ");
                  protocol.append("HTTP/1.1");
                  protocol.append("\r\n");
  
                  // http请求头
                  protocol.append("Host:");
                  protocol.append(url.getHost());
                  protocol.append("\r\n");
  
                  //空行
                  protocol.append("\r\n");
  ```
  
  ```
              //post请求体 get没有
              System.out.println("发送的http报文: \n" + protocol.toString());
  
              Socket socket = new Socket();
              socket.connect(new InetSocketAddress(url.getHost(), 80));
  
              //获得输入输出流
              BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
              BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
  
              bufferedWriter.write(protocol.toString());
              bufferedWriter.flush();
  
              StringBuilder stringBuilder = new StringBuilder();
  ```

```
              String line = "";
              while ((line = bufferedReader.readLine()) != null) {
                  stringBuilder.append(line)
                          .append("\r\n");

              }
              System.out.println(stringBuilder.toString());
```

```
* Http Post请求

参数放入请求体中

```java
                         URL url = new URL(cUrl);
            StringBuffer protocol = new StringBuffer();
            //请求行
            protocol.append("POST ");
            protocol.append(url.getPath());
            protocol.append(" ");
            protocol.append("HTTP/1.1");
            protocol.append("\r\n");

            // http请求头
            protocol.append("Host:");
            protocol.append(url.getHost());
            protocol.append("\r\n");

            protocol.append("Content-Length: 60\r\n");
            protocol.append("Content-Type: application/x-www-form-urlencoded\r\n");


            //空行
            protocol.append("\r\n"); // 必须用空行分隔请求体


            //post请求体 get没有
//                protocol.append("city="+ URLEncoder.encode("长沙","UTF-8") +"&key=13cb58f5884f9749287abbead9c658f2");
            protocol.append(url.getQuery());
            System.out.println("发送的http报文: \n" + protocol.toString());

            Socket socket = new Socket();
            socket.connect(new InetSocketAddress(url.getHost(), 80));

            //获得输入输出流
            BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            bufferedWriter.write(protocol.toString());
            bufferedWriter.flush();

            StringBuilder stringBuilder = new StringBuilder();


            String line = "";
            while ((line = bufferedReader.readLine()) != null) {
                stringBuilder.append(line)
                        .append("\r\n");

            }
            System.out.println(stringBuilder.toString());
```

### 分块编码

数据量很大！分块编码

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-07-21_3.43_chunk.png)

上面的5就是 下面的字符长度

### HTTPS

https://www.bilibili.com/video/BV1F54y1t7Dx?p=5

https://www.bilibili.com/video/BV1j7411H7vV?from=search&seid=12798261982184143495

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_10_https1.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_10_https2.png)

# 传输层 (TCP UDP)

代码运行在用户机器上，为了对 网络层进行更好的控制 UDP(User Datagram Protocol)

1. 面向报文传输

2. 没有拥塞控制,无法保证数据在网络中是否丢失

3. UDP首部开销小
   
   ​

![传输层]

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/udp_datagram.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/udp1.png)

TCP (Transmission Control Protocol)

1. 面向连接的协议

2. 提供可靠传输

3. 全双工通信

4. 面向字节流协议: 可能对用户数据合并或分拆进行传输

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp1.png)

TCP首部 

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp2.png)

序号

tcp标记

## 如何保证TCP可靠传输?

- 停止等待协议

- 连续ARQ协议(Automatic Repeat request) :
  
  ​ 滑动窗口: 以字节为单位，实现流量控制 累计确认
  
  ​ 选择重传: 重传边界和范围

- 定时器
  
  超时定时器
  
  坚持定时器: 解决死锁局面,当收到窗口为0的消息，则启动坚持定时器,每隔一段时间发送一个窗口探测报文
  
  TCP协议的拥塞控制
  
  报文超时即认为拥塞
  
  慢启动算法
  
  由小到大逐渐增加发送数据量,直到到达 “慢启动阈值”=>开始 拥塞避免算法
  
  拥塞避免算法
  
  只要网络不拥塞，就试探拥塞窗口调大

### TCP三次握手

TCP标记

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp3.png)

三次握手

![shake]![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp_handshake.png)

### 为何3次握手1

客户端角度 : 1 ，2，说明客户端的发送和接收没问题

服务端角度： 1，2，说明服务端的接收没问题，但是不确定自己的发送收否有问题，所以需要3确认

### 为何3次握手2

第一次握手客户端发出: Server觉得 : 客户端的发送能力没问题

第二次握手服务端发出: Client觉得 : 服务端的接收能力没问题， 以及服务端的发送能力没问题。

第三次握手客户端发出: Server觉得: 客户端的接收能力没问题。

**为什么要第三次握手?**

![](https://www.programmersought.com/images/29/378161939bb91ee6a16d6719b5bb381d.png)

只有两次握手的话，一旦第二次握手接收超时，重新发送后就会重新发送，会导致建立2个连接

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp_handshake2.png)

## TCP连接的4次挥手

1. 由客户端向服务端发起 : 服务端收到信息后,就能确定客户端已经停止发送数据。

2. 由服务端向客户端发起 : 客户端收到消息后,就能确定服务端已经知道客户端不会再发送数据。这一次是为了TCP可靠传输，确认收到了数据包。

3. 由服务端向客户端发起 : 客户端收到消息后,就能确定服务端已经停止发送数据。此时 服务端数据发送完了。

4. 由客户端向服务端发起 : 服务端收到信息后,就能确定客户端已经知道服务端不会再发送数据。

5. Client: 分手

6. Server : what

7. Server : 好吧 分吧

8. Client : 我已经删微信了；
- 等待计时器 : 2MSL(Max Segment liftime),

- **为什么需要等待计时器?**： 如果第4个挥手报文接收方没收到，那么接收方会重新发送第3次的挥手报文

- **为什么第3次挥手是接收方发出的?** : 我的理解是，第2次挥手是确认收到的回复，第三次是挥手是确认数据发送完毕，告诉发送方可以断开了,然而我又有了疑问?

- 为什么需要第4次挥手?

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/tcp_bye.png)

![](https://pic2.zhimg.com/80/v2-914ad7e22599fd63d2c9434d55d4b6c6_720w.jpg?source=1940ef5c)

小扎和小美通信

https://www.zhihu.com/question/63264012

# 网络层

通过ISP、路由 找到 ,运行在路由器上 rip协议:不太理解

网络配置

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_8_netconfig.png)

## 主机计算

IPADDR : 192.168.139.10

NETMASK子网掩码 : 255.255.255.0

网络号: 就是上面的IPADDR ,NETMASK进行 与运算, 192.168.139.0,10就是网络中的第10号主机。

##路由表

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_8.30_router.png)

1. 第一条意思
   
   我这台计算机通过 eth0,不需要任何中间的网关Geteway,久可以访问这个网络(192.168.150.)中的任何计算机，

## 怎么找到目标主机

面试题:路由器是怎么找目标服务器

https://www.bilibili.com/video/BV1PU4y1s7Bs?p=18&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1PU4y1s7Bs?p=24&spm_id_from=pageDriver

1. www.baidu.com通过 DNS解析得到目标ip地址 61.135.169.121

2. 61.135.169.121和第一条255.255.255.0进行 按位与运算的到的61.135.169.0和192.168.150.0结果不一样，所以该条目被淘汰。

3. 第二条目不用管（好像这个条目有问题），来到第三条，按照步骤2按位与运算，得到结果0.0.0.0，和Destination结果一样。则此时的Gateway起作用。

4. 数据给到下一跳网关，也就是192.168.150.2，如果获得的下一跳网关Gateway是 0.0.0.0的话，那么说明在同一个局域网直接发数据就好了，不用跳了。

#数据链路层

## 封装成帧

## 透明传输

## 差错检测

ARP协议

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_9_arp.png)
网卡的IP 192.168.150.1 和at后面对应的 网卡mac地址。

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/2021-08-15_arp_demo.png)

## 网卡pc1到pc5的过程

网络数据里层存放目标IP地址，外层有下一跳的mac地址。

但是mac地址是怎么来的呢?

就是通过 arp协议,

1. pc1发到pc2路由器,带的数据是1，mac先定义成FFFFFF
2. pc2接受到后，回传给pc1自己的mac地址，接着上面的传输层步骤4就有了下一跳的mac地址了。

# 物理层: 铜线介质类

1. 数据链路层 ： 相邻两台机器间的连接，把数据报以帧的形式发送, 以太网协议

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/Ethernet1.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/NETWORK/Ethernet2.png)

https://www.bilibili.com/video/BV1pA411M7Ai?p=137&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1PU4y1s7Bs?from=search&seid=16016904944562540448

https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html

https://hpbn.co/brief-history-of-http/

https://www.w3.org/Protocols/History.html

https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP

TCP 三次握手，确保两端收发能力的正常。

Client -> Server : Client发送Ok

|                 | Client send | Client recieve | Server send | Server Recieve |
| --------------- | ----------- | -------------- | ----------- | -------------- |
| Client->Server  | ok          |                |             |                |
| Server ->Client |             |                |             | ok             |
| Client->Server  |             | ok             | ok          |                |