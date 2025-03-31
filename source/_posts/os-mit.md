---
title: os_mit
date: 2025-03-22 20:25:52
tags: 
categories:  OS
---

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
