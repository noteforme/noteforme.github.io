---
title: os_mit
date: 2025-03-22 20:25:52
tags: 
categories:  OS
---

If you want to know more about page tables, and how the OS handles memory, we encourage you to take a computer organization course after completing this specialization.

https://www.coursera.org/learn/pointers-arrays-recursion/supplement/QuCc7/string-literals

# MIT OS environment

https://hub.docker.com/r/calvinhaynes412/mit6.s081

# run docker

https://docs.docker.com/reference/cli/docker/container/

```bash
docker pull calvinhaynes412/mit6.s081:v1.3.1
git clone git@github.com:BlogForMe/20labs.git

docker run -p 8848:8848 --name=mit6.s081 -u 0:0 -v "/home/m/20labs:/mit6.s081" calvinhaynes412/mit6.s081:v1.3.1
```

1. 本地用户浏览器访问 [http://localhost:8848/](https://link.zhihu.com/?target=http%3A//localhost%3A8848/) 密码： **mit6s081**.
2. 服务器端用户浏览器访问**http://<服务器公网ip>:8848/** 密码： **mit6s081**.

https://docs.docker.com/reference/cli/docker/container/

# Docker container

## run home

```bash
sudo docker container ls -a

//ho
sudo docker container start cf5ce745d947
```

https://hub.docker.com/search?q=s081&architecture=amd64

## run office

```bash
sudo docker container ls -a

//ho
sudo docker container start 1edd7b96390b
```

https://hub.docker.com/r/calvinhaynes412/mit6.s081

https://hub.docker.com/search?q=s081&architecture=amd64

```bash
docker container exec -it b66e8a0c279a /bin/bash

docker run --rm -it -v /home/m/aosp8:/aosp sabdelkader/aosp
```

当前主机 / docker容器内的目录

# lab note

排版差但是准确

https://th0ar.gitbooks.io/xv6-chinese/content/content/chapter0.html

排版好但是代码显示有误

[MIT 6.S081 Lecture Notes - Xiao Fan's Personal Page](https://fanxiao.tech/posts/2021-03-02-mit-6s081-notes/)

# code

[GitHub - ZachVec/MIT-6.S081: 6.S081 is an intermediate course in MIT for undergraduate CS students. This course involves some of the most important ideas in operating systems and provides some practice to get students familiar with those ideas.](https://github.com/ZachVec/MIT-6.S081)

可以看看berkeley的cs61c课程，做完与riscv相关的两个lab和一个project，你就会对riscv非常了解了

[](https://giscus.app/api/oauth/authorize?redirect_uri=https%3A%2F%2Fcsdiy.wiki%2F%25E6%2593%258D%25E4%25BD%259C%25E7%25B3%25BB%25E7%25BB%259F%2FMIT6.S081%2F)

debug

[MIT 6.S081 xv6调试不完全指北 - KatyuMarisa - 博客园](https://www.cnblogs.com/KatyuMarisaBlog/p/13727565.html)

[6.S081的调试和VSCode环境配置 | 止息'幻想乡

(https://zhangjk98.xyz/6.S081-VSCode-prepare-and-kernel-debugging/)

Video

阿苏EEer

https://www.bilibili.com/video/BV1VZ4y197uk?spm_id_from=333.788.videopod.sections&vd_source=d4c5260002405798a57476b318eccac9
