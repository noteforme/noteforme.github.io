---
title: os_mit
date: 2025-03-22 20:25:52
tags: 
categories:  OS
---

# MIT6.S081

[GitHub - YukunJ/xv6-operating-system: XV6 - MIT 6.s081 operating system Fall 2020 version](https://github.com/YukunJ/xv6-operating-system)

https://zhuanlan.zhihu.com/p/449687883

https://zhuanlan.zhihu.com/p/442656932

[MIT 6.S081: Operating System Engineering - CS自学指南](https://csdiy.wiki/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/MIT6.S081/)

这几位教授还专门写了一本[教程](https://pdos.csail.mit.edu/6.828/2021/xv6/book-riscv-rev2.pdf)，详细讲解了 xv6 的设计思想和实现细节。

这门课的讲授也很有意思，老师会带着学生依照 xv6 的源代码去理解操作系统的众多机制和设计细节，而不是停留于理论知识。每周都会有一个 lab，让你在 xv6 上增加一些新的机制和特性，非常注重学生动手能力的培养。整个学期一共有 11 个 lab，让你全方位地深刻理解操作系统的每个部分，非常有成就感。而且所有的lab都有着非常完善的测试框架，有的测试代码甚至上千行，让人不得不佩服 MIT 的几位教授为了教好这门课所付出的心血。

这门课的后半程会讲授操作系统领域的多篇经典论文，涉及文件系统、系统安全、网络、虚拟化等等多个主题，让你有机会接触到学界最前沿的研究方向。

## 课程资源

- 课程网站：https://pdos.csail.mit.edu/6.828/2021/schedule.html
- 课程视频： [- YouTube](https://www.youtube.com/watch?v=L6YqHxYHa7A)，每节课的链接详见课程网站
- 课程视频翻译文档：[简介 | MIT6.S081](https://mit-public-courses-cn-translatio.gitbook.io/mit6-s081/)
- 课程教材：https://pdos.csail.mit.edu/6.828/2021/xv6/book-riscv-rev2.pdf
- 课程作业：[https://pdos.csail.mit.edu/6.828/2021/schedule.html，11个lab，具体要求详见课程网站](https://pdos.csail.mit.edu/6.828/2021/schedule.html%EF%BC%8C11%E4%B8%AAlab%EF%BC%8C%E5%85%B7%E4%BD%93%E8%A6%81%E6%B1%82%E8%AF%A6%E8%A7%81%E8%AF%BE%E7%A8%8B%E7%BD%91%E7%AB%99)

[6.S081 Fall 2020 Lecture 1: Introduction and Examples - YouTube](https://www.youtube.com/watch?v=L6YqHxYHa7A)

If you want to know more about page tables, and how the OS handles memory, we encourage you to take a computer organization course after completing this specialization.

https://www.coursera.org/learn/pointers-arrays-recursion/supplement/QuCc7/string-literals

# MIT OS environment

https://hub.docker.com/r/calvinhaynes412/mit6.s081

# config docker

https://docs.docker.com/reference/cli/docker/container/

```bash
docker pull calvinhaynes412/mit6.s081:v1.3.1
git clone git@github.com:BlogForMe/20labs.git

docker run -p 8848:8848 --name=mit6.s081 -u 0:0 -v "/home/m/20labs:/mit6.s081" calvinhaynes412/mit6.s081:v1.3.1


//home mac config docker 
docker run -p 8848:8848 --name=mit6.s081 -u 0:0 -v "/Users/m/Documents/MIT6S081/20labs:/mit6.s081" calvinhaynes412/mit6.s081:v1.3.1


sudo docker container start 2d078986b704
```

"/Users/m/Documents/MIT6S081/20labs" is local directory in mac

本地用户浏览器访问 [http://localhost:8848/](https://link.zhihu.com/?target=http%3A//localhost%3A8848/) 密码： **mit6s081**.

服务器端用户浏览器访问**http://<服务器公网ip>:8848/** 密码： **mit6s081**.

https://docs.docker.com/reference/cli/docker/container/

# Docker container

## run home

```bash
sudo docker container ls -a

//home mac
sudo docker container start 2d078986b704

//ho ubun
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

process

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>

int main() {
    printf("execute: before\n");
    int pid = fork();
    printf("execute: order =%d\n", pid);

    if (pid > 0) {
        // Parent process
        printf("parent: child=%d\n", pid);
        pid = wait((int *) 0);
        printf("child %d is done\n", pid);
    } 
    else if (pid == 0) {
        // Child process
        printf("child: exiting\n");
        exit(0);  // ← CHILD TERMINATES HERE!
    } 
    else {
        // Fork failed
        printf("fork error\n");
        return 1;
    }

    printf("execute: after\n");  // ← Only parent reaches this
    return 0;
}
```

why child cannot reach  printf("execute: after\n")

# pipe

一个管道，parent 和child 可以操作。

如果parent ， child  都要写入和读取 ，那么就建立 两个管道，一个读取，一个写入。

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/os/202510021106588.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/os/202510021111676.png)

[OS24 - Pipes | Interprocess Communication - YouTube](https://www.youtube.com/watch?v=s-cBPllAYD8&t=39s)

## **Day 1：进程隔离**

- **上午（OS）**
  
  - MIT 6.S081 Lecture 2-3：进程 & 系统调用
  
  - Lab1: Shell → 用 `fork/exec/wait` 实现简易 Shell

- **下午（云原生）**
  
  - 学习 Linux namespace（uts、ipc、net、pid）
  
  - 实践：
    
    `sudo unshare --uts --ipc --net --pid --fork bash`
    
    进入隔离环境，修改主机名、查看 PID

- **收获**：理解容器 = 被隔离的进程

### 🔹 你刚刚完成了 Day1 云原生实验的关键结论

- **容器不是虚拟机**，而是 **Linux 进程在新的 namespace 里的视图**。

- 在 namespace 里，它“以为”自己是 **系统的 PID=1**，但宿主机上它其实只是个普通进程（比如 PID=2365）。

- 这正是 Docker/Kubernetes 的底层机制。

---

### 🔹 建议你记录在实验报告里

写上：

1. 进入 namespace 的命令：
   
   `sudo unshare --uts --ipc --net --pid --mount-proc --fork bash`

2. 输出结果：
   
   - `ps -ef` → bash PID=1
   
   - `/proc/1/cmdline` → bash

3. 实验结论：**容器 = 被 namespace 隔离的进程**。

---

## **Day 2：调度原理**

- **上午（OS）**
  
  - MIT 6.S081 Lecture 9：调度算法
  
  - Lab3: Scheduler → 给 xv6 加调度策略

- **下午（云原生）**
  
  - 学 Kubernetes 调度流程（Filter → Score → Bind）
  
  - `kubectl get events` 观察 Pod 调度

- **收获**：K8s 调度器 ≈ 操作系统调度器

---

## **Day 3：资源控制**

- **上午（OS）**
  
  - 学习 cgroups（CPU/内存/IO 控制）
  
  - 实验：
    
    `cgcreate -g memory:/mygroup cgexec -g memory:/mygroup stress --vm 1 --vm-bytes 200M`

- **下午（云原生）**
  
  - Docker 资源限制：
    
    `docker run -d -m 100m --cpus=0.5 busybox top`
  
  - K8s Pod 限制：`resources.requests/limits`

- **收获**：容器资源限制就是 OS cgroups 的封装

---

## **Day 4：文件系统**

- **上午（OS）**
  
  - MIT 6.S081 Lecture 7-8：文件系统
  
  - Lab5: File System → 文件挂载实验

- **下午（云原生）**
  
  - 学 OverlayFS（Docker 镜像原理）
  
  - K8s Volume 实验：挂载 ConfigMap/HostPath

- **收获**：容器镜像和 Volume ≈ 文件系统隔离

---

## **Day 5：并发与控制器**

- **上午（OS）**
  
  - MIT 6.S081 Lecture 10-11：并发、锁
  
  - Lab4: Thread → 多线程与同步

- **下午（云原生）**
  
  - 学 Go 并发（goroutine + channel）
  
  - 写个 Client-Go 小程序：定时列出集群 Pod

- **收获**：K8s 控制器本质是“并发循环 + 状态修复”

---

## **Day 6：Kubernetes 架构**

- **上午（OS）**
  
  - 总结 OS 三大支柱：进程、内存、文件系统

- **下午（云原生）**
  
  - 学 Kubernetes 核心组件：API Server、etcd、Scheduler、Controller、Kubelet
  
  - 用 Minikube 部署三层应用（前端+后端+DB）

- **收获**：K8s ≈ 分布式操作系统

---

## **Day 7：综合实战**

- **上午（OS）**
  
  - 快速复盘 Lab1 / Lab3 / Lab5 收获

- **下午（云原生）**
  
  - 写一个简单的 K8s 控制器（Go + client-go）  
    示例：自动清理运行超过 1 小时的 Pod

- **收获**：打通 OS 原理 → 容器 → Kubernetes → 平台研发的闭环

---

✅ 最终你会收获：

- MIT 6.S081 的核心实验经验

- Linux namespace + cgroups 实操

- Docker & Kubernetes 实战部署

- 一个 **自定义控制器 demo 项目**（能放 GitHub/简历）
