---
title: ubuntu 安装 shadowsocks  ssr
date: 2017-07-18 14:34:53
tags:
categories: "LINUX"

---

  自从shadowsocks出来，我们的上网问题算是解决了，通过google查资料确实更便利

# 一、shadowsocks安装
   对于新手 作者建议安装在ubuntu上，那就用他开始吧
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

_ _ _
 # 二、SSR配置

刚写完上面没多久  有消息说深圳启动了ss检测系统，为了避免tea，还是换了算了，现在都是用别人的，有时间也看看是咋回事
  
  开始吧
  
 1.   获取源代码
  
              git clone -b manyuser https://github.com/shadowsocksr/shadowsocksr.git
   
       在当前位置 ls ,可以看到 shadowsocksr目录,使用下面的任务
      

            cd ~/shadowsocksr
            bash initcfg.sh

    
2.  然后ls 又可以看到 shadowsocks,接着执行
      
         python server.py -p 8888 -k 111111 -m aes-256-cfb -O auth_sha1_v4 -o tls1.2_ticket_auth 
  注意： 上面端口如果用443，貌似连不上，我的是这样的

     客户端按照这个配置就可以了
     ,后台运行
     
         python server.py -p 8888 -k 111111 -m aes-256-cfb -O auth_sha1_v4 -o tls1.2_ticket_auth  -d start
    停止或重启
       
         python server.py -d stop/restart
查看日志：
              
         tail -f /var/log/shadowsocksr.log
3 . 使用配置文件运行 在shadowsocksr目录下,修改user-config.json,这五项就可以了，其他是拓展用的
      "server_port":8388,        //端口
     "password":"password",     //密码
      "protocol":"origin",       //协议插件
      "obfs":"http_simple",      //混淆插件
      "method":"aes-256-cfb",    //加密方式
  然后cd  shadowsocnk 进入该目录后
    
          python server.py
  如果要在后台运行：
  
          python server.py -d start

  如果要停止/重启：

    
         python server.py -d stop/restart
