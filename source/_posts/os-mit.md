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

# 系统调用

系统调用流程

[Lab2:系统调用 - IukBlog](https://kevin-aron.github.io/categories/mit6.s081/Lab2-%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8/)

[[mit6.s081]Lab2: System calls | 系统调用_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV129XuYGEjT/?spm_id_from=333.337.search-card.all.click&vd_source=d4c5260002405798a57476b318eccac9)

[MIT6.s081操作系统: lec6 trap陷阱机制 走进system call的前世今生 课程导读和源码浅析_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1rv421y7ta?spm_id_from=333.788.player.switch&vd_source=d4c5260002405798a57476b318eccac9)



讲解详细的调用流程



$ trace 32 grep hello README

```c
// System call numbers
#define SYS_fork    1
#define SYS_exit    2
#define SYS_wait    3
#define SYS_pipe    4
#define SYS_read    5
```

32 = 2的5次方 ， 所以是追踪 read系统调用。从README文件中找hello字符串。

## System call tracing

In the first example above, trace invokes grep tracing just the read system call. The 32 is 1<<SYS_read. In the second example, trace runs grep while tracing all system calls; the 2147483647 has all 31 low bits set. In the third example, the program isn't traced, so no trace output is printed. In the fourth example, the fork system calls of all the descendants of the forkforkfork test in usertests are being traced. Your solution is correct if your program behaves as shown above (though the process IDs may be different).

Some hints:

- Add $U/_trace to UPROGS in Makefile
  
  ```
  UPROGS=\
      $U/_cat\
      $U/_usertests\
      $U/_grind\
      $U/_wc\
      $U/_zombie\
      $U/_trace\
  ```

- Run make qemu and you will see that the compiler cannot compile user/trace.c, because the user-space stubs for the system call don't exist yet: add a prototype for the system call to user/user.h, a stub to user/usys.pl, and a syscall number to kernel/syscall.h. The Makefile invokes the perl script user/usys.pl, which produces user/usys.S, the actual system call stubs, which use the RISC-V ecall instruction to transition to the kernel. Once you fix the compilation issues, run trace 32 grep hello README; it will fail because you haven't implemented the system call in the kernel yet.

- Add a sys_trace() function in kernel/sysproc.c that implements the new system call by remembering its argument in a new variable in the proc structure (see kernel/proc.h). The functions to retrieve system call arguments from user space are in kernel/syscall.c, and you can see examples of their use in kernel/sysproc.c.

- Modify fork() (see kernel/proc.c) to copy the trace mask from the parent to the child process.

- Modify the syscall() function in kernel/syscall.c to print the trace output. You will need to add an array of syscall names to index into.





## Sysinfo
