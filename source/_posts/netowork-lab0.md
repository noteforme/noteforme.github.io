---
id: 387
title: 'Netowork Lab0'
date: '2024-06-08T01:36:41+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=387'
permalink: /2024/06/08/netowork-lab0/
categories:
    - NETWORK
---



# environment

https://web.stanford.edu/class/cs144/vm_howto/
username , pwd
cs144, cs144

## virtualbox m1 download

https://www.virtualbox.org/wiki/Download_Old_Builds_7_0

## visual studio

Checkpoint 0: networking warmup ([intro vide](https://stanford.zoom.us/rec/play/osIlsm92QUIM9xrJ4jpW7gn1BpsiJ2d7yFiE90sozEv-ZYatCinSnn8Huxe3yQQE8-8EvdPa6TlxJ7g5.lPlgTIJvn6FdU3tO?canPlayFromShare=true&from=share_recording_detail&continueMode=true&componentName=rec-play&originRequestUrl=https%3A%2F%2Fstanford.zoom.us%2Frec%2Fshare%2F2C8HSVHZcTLlbvLZSd1qj5u7l5RfS-OBo2liMTjvRTcRL-g87VCdsna1fKixRb0H.AfzvlgwJA0C6eGx1 "intro vide")o)

## connect to VM

ssh connect server

```bash
ip -a
ssh cs144@192.168.64.2
```

# LAB0

## Fetch a Web page

### hello

#### In a Web browser,

    visit http://cs144.keithw.org/hello and observe the result.

#### do the same thing the browser does, by hand.

1. On your VM, run telnet cs144.keithw.org http . This tells the telnet
   program to open a reliable byte stream between your computer and another
   computer (named cs144.keithw.org), and with a particular service running onthat computer: the “http” service, for the Hyper-Text Transfer Protocol, used by
   the World Wide Web.1
   If your computer has been set up properly and is on the Internet, you will see:
   user@computer:~$ telnet cs144.keithw.org http
   Trying 104.196.238.229...
   Connected to cs144.keithw.org.
   Escape character is '^]'.
   If you need to quit, hold down ctrl and press ] , and then type close .
2. Type GET /hello HTTP/1.1 . This tells the server the path part of the URL.
   (The part starting with the third slash.)
3. Type Host: cs144.keithw.org . This tells the server the host part of the
   URL. (The part between http:// and the third slash.)
4. Type Connection: close . This tells the server that you are finished
   making requests, and it should close the connection as soon as it finishes replying.
5. Hit the Enter key one more time: . This sends an empty line and tells the
   server that you are done with your HTTP request.
6. If all went well, you will see the same response that your browser saw, preceded
   by HTTP headers that tell the browser how to interpret the response

#### my lab1

Notice, don't wait for a timeout when you input 'telnet cs144.keithw.org http'.

```bash
cs144@vm:~$ telnet cs144.keithw.org http 
Trying 104.196.238.229...
Connected to cs144.keithw.org.
Escape character is '^]'.
GET /hello HTTP/1.1
Host: cs144.keithw.org
Connection: close             //after inputing all the above commands

HTTP/1.1 200 OK
Date: Mon, 10 Jun 2024 03:18:22 GMT
Server: Apache
Last-Modified: Thu, 13 Dec 2018 15:45:29 GMT
ETag: "e-57ce93446cb64"
Accept-Ranges: bytes
Content-Length: 14
Connection: close
Content-Type: text/plain

Hello, CS144!
Connection closed by foreign host.
```

#### Assignment

http://cs144.keithw.org/lab0/sunjohn

```bash
cs144@vm:~$ telnet cs144.keithw.org http
GET /lab0/sunjohn HTTP/1.1
Host: cs144.keithw.org
Connection: close             //after inputing all the above commands

HTTP/1.1 200 OK
Date: Mon, 10 Jun 2024 03:59:20 GMT
Server: Apache
X-You-Said-Your-SunetID-Was: sunjohn
X-Your-Code-Is: 174534
Content-length: 111
Vary: Accept-Encoding
Content-Type: text/plain

Hello! You told us that your SUNet ID was "sunjohn". Please see the HTTP headers (above) for your secret code.
```

Send yourself an email
这个应该需要stanford内网,应该不用做

目前课程学到lab0
3.2 Modern C++: mostly safe but still fast and low-level
部分，还是很有意思,后面有时间继续后续的学习.