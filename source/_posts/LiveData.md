---
title: LiveData
comments: true
date: 2020-06-15 11:20:42
tags:
categories: Architecture
---



数据倒灌

> 经过了解和使用 view model,databiding后，我觉得这个不是官方留的坑，在特定的场景可能还需要这样做，我获取手环睡眠数据，此时有其他fragment还没创建，我就需要创建后才就监听到数据。

对于这个问题官方推荐 singleLiveEvent

或者 ELiveData 

https://www.ershicimi.com/p/2f57df7148300017e8a0f7227720a2e8



https://juejin.im/post/5b2b1b2cf265da5952314b63



https://www.jianshu.com/p/79d909b6f8bd

https://juejin.im/post/6892704779781275662