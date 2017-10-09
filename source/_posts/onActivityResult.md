---
title: onActivityResult
comments: true
date: 2017-09-26 14:01:53
tags:
categories: ANDROID

---

onActivityResult方法用的算比较普遍，刚好有个困惑，回传数据时`intent.putExtra("text",122);`  
使用 `  String couponId = data.getStringExtra("text");` 接收时为null,里面跳转和回传的
内部机制是怎么样的？？？

