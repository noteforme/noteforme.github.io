---
title: PrincipleComputer
comments: true
date: 2019-04-08 15:52:07
tags:
categories: 计算机组成原理
---



做蓝牙开发时byte数组出现 -122，来看看这个负数是怎么出现的

byte转int类型

b1 & 0xff

<http://ju.outofmemory.cn/entry/215778>

<https://blog.csdn.net/RuobaiMEN/article/details/79890823>

<https://blog.csdn.net/LVXIANGAN/article/details/72726152>

问题: 计算机负数加法 8 + （-1）

​	b1 & 0xff 不理解

<https://www.imooc.com/article/21360>

<https://blog.csdn.net/zdy10326621/article/details/50236529>

#### 字节

###### 一个字节byte 两位16进制数

1个字节是8位，二进制8位：xxxxxxxx 范围从00000000－11111111，表示0到255。

1位16进制数最大是15（用二进制表示是xxxx ）（即对应16进制的0xF 1111 4位），要表示到255,就还需要4位。

* 位运算符基本操作