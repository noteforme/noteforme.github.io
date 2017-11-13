---
title: ubuntu 安装 ssr
date: 2017-07-18 14:34:53
tags:
categories: "LINUX"

---

  自从shadowsocks出来，我们的上网问题算是解决了，通过google查资料确实更便利

# shadowsocks安装
   对于新手 作者建议安装在ubuntu上，那就用他开始吧
##  安装软件
  
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

     
## 前台运行
 软件安装好了，现在可以配置了，先在前台跑下

     
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

# 卸载shadowsocks
　
　先停掉服务 `sudo ssserver -d stop`
　然后　      `pip uninstall shadowsocks`
　
　

_ _ _

 # SSR配置

刚写完上面没多久  有消息说深圳启动了ss检测系统，为了避免tea，还是换了算了，现在都是用别人的，有时间也看看是咋回事
  
  开始吧
  
##  获取源代码
  
              git clone -b manyuser https://github.com/shadowsocksr/shadowsocksr.git

       在当前位置 ls ,可以看到 shadowsocksr目录,使用下面的任务


            cd ~/shadowsocksr
            bash initcfg.sh


 然后ls 又可以看到 shadowsocks,接着执行

         python server.py -p 8888 -k 111111 -m aes-256-cfb -O auth_sha1_v4 -o tls1.2_ticket_auth
  注意： 上面端口如果用443，貌似连不上，我的是这样的

     客户端按照这个配置就可以了 ,后台运行
```
         python server.py -p 8888 -k 111111 -m aes-256-cfb -O auth_sha1_v4 -o tls1.2_ticket_auth  -d start
```

停止或重启


         python server.py -d stop/restart
         tail -f /var/log/shadowsocksr.log  //查看日志

## 配置参数
  使用配置文件运行 在shadowsocksr目录下,修改user-config.json,这五项就可以了，其他是拓展用的

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

## 配置ubuntu开机启动
*  home下　新建 run_server.sh
输入

```
#!/bin/bash
cd /home/jon/shadowsocksr/shadowsocks
python local.py -c /etc/shadowsocks.json
```
 * 修改脚本权限
 一定要让脚本具备可执行权限，可以执行如下指令：
`$ sudo chmod 755 run_server.sh`

* 将脚本放置在启动路径下
将run_server.sh移动到/etc/init.d路径下，可以直接拷贝，也可以链接过去
`$ sudo cp run_server.sh /etc/init.d/`

* 将脚本添加到启动脚本。
执行如下指令，在这里90表明一个优先级，越高表示执行的越晚

```
$ cd /etc/init.d/
$ sudo update-rc.d run_server.sh defaults 90
```

* 如何移除该脚本
很简单，执行如下指令：
` sudo update-rc.d -f run_server.sh remove`
 

http://jackqdyulei.github.io/2016/03/06/linux-auto-script/

#  shadowsocks-libev obfs

另一种形式的shadowsocks
 

##  shadowsocks-libev安装

ubuntu 17

```
sudo apt update
sudo apt install shadowsocks-libev
```
from :https://github.com/shadowsocks/shadowsocks-libev#install-from-repository

##  启动测试连接

和shadowsocks启动类似 ssserver 换成了 ss-server

* 启动测试 : `ss-server -s 0.0.0.0 -p 8388 -k 123456 -m aes-256-cfb`　　没有 `-d start`

* 配置文件启动
  To run in the foreground: `ss-server -c /etc/shadowsocks-libev/config.json `
* To run in the background:　
  ｀nohup ss-server -c /etc/shadowsocks-libev/config.json｀


##  simple-obfs安装

Build

For Unix-like systems, especially Debian-based systems, e.g. Ubuntu, Debian or Linux Mint, you can build the binary like this:

* Debian / Ubuntu　环境配置
`sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libc-ares-dev libev-dev asciidoc xmlto automake `

* 下载simple-obfs
```
git clone https://github.com/shadowsocks/simple-obfs.git
cd simple-obfs
git submodule update --init --recursive
./autogen.sh
./configure && make
sudo make install
```
from  https://github.com/shadowsocks/simple-obfs

##  注意:
* 问题1
`autoconf --version`
需要安装 `sudo apt install autoconf`

* 问题2
```
configure.ac:19: error: possibly undefined macro: AC_DISABLE_STATIC
      If this token and others are legitimate, please use m4_pattern_allow.
      See the Autoconf documentation.
configure.ac:20: error: possibly undefined macro: AC_DISABLE_SHARED
configure.ac:44: error: possibly undefined macro: AC_PROG_LIBTOOL
autoreconf: /usr/bin/autoconf failed with exit status: 1
```
`sudo apt-get install libtool`

*  问题3
![autogen.sh](ubuntu_ssr/shadowsocklib_autogen.png)

`sudo apt-get install automake`
http://ask.xmodulo.com/fix-failed-to-run-aclocal.html

## 混淆

* server端: `ss-server -c  /etc/shadowsocks-libev/config.json  --plugin obfs-server --plugin-opts "obfs=http"`

* clinet 端:`ss-local -c config.json --plugin obfs-local --plugin-opts "obfs=http;obfs-host=www.bing.com"
`
也可以都放在配置文件中

```
{
    "server":"0.0.0.0",
    "server_port":your_server_port,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"your_password",
    "timeout":300,
    "method":"your_encryption_method",
    "plugin":"/usr/local/bin/obfs-server --obfs http"
}
```
那么服务端直接运行``ss-server -c /etc/shadowsocks-libev/config.json `就可以了

后台运行就是前面的测试连接 nohup命令　

from:https://teddysun.com/511.html


参考:https://github.com/shadowsocks/simple-obfs
https://blog.phpgao.com/shadowsocks_on_linux.html
https://softwaredownload.gitbooks.io/openwrt-fanqiang/content/ebook/03.2.html


   
  
 
  
  
  





