---
title: Docker_Config
date: 2025-02-16 22:33:52
tags:
categories: TOOL
---

# PROXY

https://docs.docker.com/engine/daemon/proxy/

vim /etc/docker/daemon.json

```json
{
  "proxies": {
    "http-proxy": "http://127.0.0.1:7897",
    "https-proxy": "http://127.0.0.1:7897",
    "no-proxy": "*.test.example.com,.example.org,127.0.0.0/8"
  }
}
```

sudo mkdir -p /etc/systemd/system/docker.service.d
/etc/systemd/system/docker.service.d/http-proxy.conf

```json
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7897"
Environment="HTTPS_PROXY=http://127.0.0.1:7897"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
```

# IMAGE

```bash
docker images
```

traefik                     v2.11     d7d7095a482f   2 weeks ago     178MB
phpmyadmin                  latest    834a66ad6967   3 weeks ago     570MB
hello-world                 latest    74cc54e27dc4   3 weeks ago     10.1kB
mysql                       8.0       6616596982ed   3 weeks ago     764MB
docker/welcome-to-docker    latest    c1f619b6477e   15 months ago   18.6MB

```bash
docker rmi traefik:v2.11
```

Untagged: traefik:v2.11
Deleted: sha256:d7d7095a482f62a90d32807bd051fbd28763eaf0085ebe210dd2d4b3bf3e9660

 IMAGE ID

```bash
docker rmi 834a66ad6967
```

Untagged: phpmyadmin:latest
Untagged: phpmyadmin@sha256:b8e9de0186fe7e12e3a9565432c9faf6e8f0ec0f78f07bc3625910fd130afb99

# Container

https://docs.docker.com/reference/cli/docker/container/

```bash
docker run -p 8848:8848 --name=mit6.s081 -u 0:0 -v "/home/m/20labs:/mit6.s081" calvinhaynes412/mit6.s081:v1.3.1
```

https://docs.docker.com/reference/cli/docker/container/

run docker 

```
docker container ls -a

//ho
docker container start cf5ce745d947
```

https://hub.docker.com/r/calvinhaynes412/mit6.s081

https://hub.docker.com/search?q=s081&architecture=amd64

```bash
docker container exec -it b66e8a0c279a /bin/bash

docker run --rm -it -v /home/m/aosp8:/aosp sabdelkader/aosp
```



当前主机 / docker容器内的目录
