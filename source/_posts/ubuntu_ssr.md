---
title: ubuntu 安装 ssr
date: 2017-07-18 14:34:53
tags:
categories: "LINUX"

---

  自从shadowsocks出来，我们的上网问题算是解决了，通过google查资料确实更便利

##  shadowsocks安装

   对于新手 作者建议安装在ubuntu上，那就用他开始吧

*   安装软件

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


### 前台运行

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
​     
​        ssserver -c /etc/shadowsocks.json -d start    
​        ssserver -c /etc/shadowsocks.json -d stop 

接着就可以愉快的玩耍了

*  卸载shadowsocks

　先停掉服务 `sudo ssserver -d stop`
　然后　      `pip uninstall shadowsocks`
　
　
_ _ _


##  shadowsocks-libev安装

ubuntu 17

```
sudo apt update
sudo apt install shadowsocks-libev
```
如果是其他版本可以看下面　参考
from :https://github.com/shadowsocks/shadowsocks-libev#install-from-repository

##  启动测试连接

和shadowsocks启动类似 ssserver 换成了 ss-server

* 启动测试 : `ss-server -s 0.0.0.0 -p 8388 -k 123456 -m aes-256-cfb`　　没有 `-d start`
  可能会占用端口
* 配置文件启动
  To run in the foreground: `ss-server -c /etc/shadowsocks-libev/config.json `
* To run in the background:　
  ｀nohup ss-server -c /etc/shadowsocks-libev/config.json｀
* 停止运行: `sudo ss-server -d stop`

##  simple-obfs(混淆工具)安装

Build
For Unix-like systems, especially Debian-based systems, e.g. Ubuntu, Debian or Linux Mint, you can build the binary like this:

* Debian / Ubuntu　环境配置
`sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libc-ares-dev libev-dev asciidoc xmlto automake `

* 下载simple-obfs

    //Debian / Ubuntu
    ​	sudo apt-get install --no-install-recommends build-essential autoconf libtool 			libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake
    ​			//CentOS / Fedora / RHEL
    ​		sudo yum install gcc autoconf libtool automake make zlib-devel openssl-devel asciidoc xmlto
    ​		// Arch
    ​	sudo pacman -Syu gcc autoconf libtool automake make zlib openssl asciidoc xmlto
    ​	 //Alpine
    ​	apk add gcc autoconf make libtool automake zlib-devel openssl asciidoc xmlto libpcre32 libev-dev g++ linux-headers

    	git clone https://github.com/shadowsocks/simple-obfs.git
    	cd simple-obfs
    	git submodule update --init --recursive
    	./autogen.sh
    	./configure && make
    	sudo make install


​    	 

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

* clinet 端:`ss-local -c config.json --plugin obfs-local --plugin-opts "obfs=http;obfs-host=www.baidu.com"
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

* 参数: 
```
  插件：obfs-local
  插件选项：obfs=http;obfs-host=www.baidu.com
```

* 客户端连接
　client也按照 simple-obfs  服务端安装方式安装
　连接命令 ` ss-local -c /etc/shadowsocks-libev/config.json --plugin obfs-local --plugin-opts "obfs=http;obfs-host=www.biadu.com"`


参考:https://github.com/shadowsocks/simple-obfs
https://blog.phpgao.com/shadowsocks_on_linux.html
https://softwaredownload.gitbooks.io/openwrt-fanqiang/content/ebook/03.2.html



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

 ## 设置系统全局代理

 https://blog.csdn.net/u012810317/article/details/52139361

* terminal代理

  ```
  export http_proxy=http://127.0.0.1:8118
  export https_proxy=http://127.0.0.1:8118
  ```

  或者

  git config --global http.proxy "localhost:1080"

  git config --global http.proxy "localhost:1080"



   git clone --recurse-submodules git@github.com:shadowsocks/shadowsocks-android.git



##### 修改包名

* 把apk改成 shadowsocks.apk

  ```
  ![SS_20190DD](E:\noteforme.github.io\source\_posts\ubuntu_ssr\SS_20190DD.png)java -jar apktool.jar d shadowsocks.apk      //解包
  											//中间做修改
  java -jar apktool.jar b shadowsocks			//打成apk再dist目录下
  
  jarsigner -verbose -keystore androidTest.jks -signedjar signed.apk AndroidTest.apk test  //生成签名
  
  jarsigner -verbose -keystore androidTest.jks -signedjar signed.apk shadowsockstt.apk test
  ```

  https://blog.csdn.net/huaiyiheyuan/article/details/53114490

  

##### windows全局代理

<https://www.flyzy2005.com/fan-qiang/shadowsocks/proxifier-with-shadowsocks/>

![autogen.sh](ubuntu_ssr/SS_20190DD.png)