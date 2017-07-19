---
title: ubuntu 安装 shadowsocks 
date: 2017-07-18 14:34:53
tags:
---

自从shadowsocks出来，我们的上网问题算是解决了，通过google查资料确实更便利

一、对于新手 作者建议安装在ubuntu上，那就用他开始吧
1.  安装软件
  
         sudo apt-get install python-pip
         pip install shadowsocks
  
  然而我遇到了这样的问题
  Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-Mlx8an/shadowsocks/
You are using pip version 8.1.1, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
接着按照他的指示来
  
    pip install --upgrade pip

  然而又出问题了，这次是这个 Could not import setuptools which is required to install from a source distribution.
Please install setuptools.
  这时候Google派上用场了，解决方案是 
  
  
     sudo pip install -U setuptools
 
  
  参考：https://github.com/fredley/play-pi/issues/22
  
  然后执行 pip install shadowsocks ，终于OK，Collecting shadowsocks
  Using cached shadowsocks-2.8.2.tar.gz
Installing collected packages: shadowsocks
  Running setup.py install for shadowsocks ... done
Successfully installed shadowsocks-2.8.2

     
2.软件安装好了，现在可以配置了，先在前台跑下 

     
            ssserver -p 8388 -k 123456 -m aes-256-cfb
    
 
 然后手机或电脑同样配置看下代理是否有用，Ok的话，新建一个文件替代命令  
   
  
      vi  /etc/shadowsocks.json 
      {
      "server":"my_server_ip",
      "server_port":8388,
      "local_address": "127.0.0.1",
      "local_port":1080,
      "password":"123456",
      "timeout":300,
      "method":"aes-256-cfb",
      "fast_open": false
     }



   前台执行 :   
   
        ssserver -c /etc/shadowsocks.json
 
 后台执行:
     
        ssserver -c /etc/shadowsocks.json -d start    
        ssserver -c /etc/shadowsocks.json -d stop 

接着就可以愉快的玩耍了