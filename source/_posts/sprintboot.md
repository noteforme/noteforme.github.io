---
title: sprintboot
date: 2024-11-02 15:50:50
tags: BE
---







# 黑马程序员SpringBoot教程

https://www.bilibili.com/video/BV1Lq4y1J77x



# 入门



 pom依赖

```
<!--SpringBoot工程需要继承的父工程-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.4</version>
</parent>

<dependencies>
    <!--web开发的起步依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
————————————————

                            版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
                        
原文链接：https://blog.csdn.net/qq_45966440/article/details/120450302
```

ide  自动导入依赖不是上面的，会有问题.
