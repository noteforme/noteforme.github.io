---
title: Git-sshkey
comments: true
date: 2017-09-14 18:20:17
tags:
categories: GIT

---
# 原理

http://skypegnu1.blog.51cto.com/8991766/1641064

#  多平台配置SSH

1.  用户信息:在config后加上 --global 即可全局设置用户名和邮箱，否则就是局部的。

>  $ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com

　　检查配置信息　`git config --list`


2. 生成sshkey,如果视其他平台生成时就要修改名称了

>ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
> Enter file in which to save the key (/home/jon/.ssh/id_rsa): /home/jon/.ssh/id_rsa.Oschina
>cat ~/.ssh/id_rsa.li | clip


3.  Adding your SSH key to the ssh-agent
  

#  start the ssh-agent in the background
    
      $ eval $(ssh-agent -s)
        Agent pid 59566
      $ ssh-add ~/.ssh/id_rsa.li
      
4.  gitbash下把生成的id_rsa.pub 添加到sshkey
 oschina验证ssh
　` 　　　
　　　　$ ssh -T git@git.oschina.net
`

　　github 测试连接
  
       $ ssh -T git@github.com

　      然后就可以使用ssh了
　　然后出现:
  >Hi username! You've successfully authenticated, but GitHub does not
  provide shell access.

　配置基本成功了，我前面的key重新命名，clone项目还是有问题
  
  解决方法: 在 .ssh目录下　新建config文件 添加
  
>   Host github
    HostName github.com
    User Jon
    IdentityFile ~/.ssh/id_rsa.li


参考：
github生成方式
https://help.github.com/articles/about-ssh/

gitlab生成方式
https://gitlab.com/help/ssh/README

oschina方式:
http://git.mydoc.io/?t=154712

# 　问题

1.  类似下面错误使用　git push -u origin master

> ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/aniruddhabarapatre/learn-rails.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing

参考：https://stackoverflow.com/questions/20939648/issue-pushing-new-code-in-github

2.  生成key时，我改成了　id_rsaOschina
android studio 最后push 时
authentication id_rsaOschina　using key failed报错
方法: AndroidStudio -> File -> Settings ->Git -> SSH executable : built-in 改成Native

参考:https://stackoverflow.com/questions/24688700/android-studio-push-failed-fatal-could-not-read-from-remote-repository



