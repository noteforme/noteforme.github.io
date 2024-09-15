---
id: 270
title: Architecture
date: '2024-05-30T02:01:47+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=270'
permalink: /2024/05/30/architecture/
categories:
    - Architecture
    - DesignPattern
---



# Archtechtrue

https://juejin.im/post/5e801a49e51d45470e2bc8bf

https://wanandroid.com/blog/show/2754

https://mp.weixin.qq.com/s/AJ4QUqdYMeQWgWEQAFPsww

https://juejin.im/post/6876968255597051917

#### APP模式 MVC MVP. MVVM:

面试题

- mvp与mvvm的区别，mvvm怎么更新UI

- 讲讲mvc,mvp模式，presenter内存泄漏的问题

- 代理模式与装饰模式的区别，手写一个静态代理，一个动态代理

- MVP怎么处理内存泄漏

- **Mvp与Mvvm有什么区别?**

- mvvm双向数据绑定的原理是怎样的？ViewModel

- 你在项目中有用到什么设计模式吗？

- 单例模式有什么缺点？

- 动画里面用到了什么设计模式？

- OkHttp里面用到了什么设计模式？

- 谈谈设计模式，你了解多少，运用了多少？

- MVP
  
  Presenter 的作用类似于MVC中的Controller,但是其会反作用与View层，Model层的数据更新会被首先反馈到
  
  Presenter,由Presenter优先处理并决定是否刷新以及刷新哪个View,也就是说Presenter完全将View层与Model层
  
  隔离开，充当一个名副其实的中间人角色.

- MVVM
  
  MVVM则是思想的完全变革。它是将“数据模型数据双向绑定”的思想作为核心，因此在View和Model之间没有联系，通过ViewModel进行交互，而且Model和ViewModel之间的交互是双向的，因此视图的数据的变化会同时修改数据源，而数据源数据的变化也会立即反应到View上
  
  缺点
  
  1、过于简单的图形界面不适用，或说牛刀杀鸡。
  2、对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高。
  3、数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的。

Jetpack

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/Architecture/2021-08-02_8.15_jetpack.png)

It helps to be familiar with software architectural patterns that separate data from the user interface, such as MVP or MVC

https://developer.android.com/jetpack/docs/getting-started

- modules should interact

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/Architecture/final-architecture.png)

- NetworkBoundResource

It starts by observing the database for the resource. When the entry is loaded from the database for the first time, `NetworkBoundResource` checks whether the result is good enough to be dispatched or that it should be re-fetched from the network. Note that both of these situations can happen at the same time, given that you probably want to show cached data while updating it from the network.

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/Architecture/network-bound-resource.png)

https://developer.android.com/topic/libraries/architecture

架构学习从codelab开始吧!

https://www.jianshu.com/p/19f97654c451

https://github.com/qingmei2/MVVM-Architecture

https://juejin.im/post/5dafc49b6fb9a04e17209922

https://juejin.im/post/5d2be05ff265da1bd605d49a

###### **LifecycleOwner**

[`LifecycleOwner` is an interface ](https://developer.android.com/reference/androidx/lifecycle/LifecycleOwner.html)implemented by the [`AppCompatActivity`](https://developer.android.com/reference/androidx/appcompat/app/AppCompatActivity) and [`Fragment`](https://developer.android.com/reference/androidx/fragment/app/Fragment) classes. You can subscribe other components to owner objects which implement this interface, to observe changes to the lifecycle of the owner.

To read an introductory guide to this topic, see [Handling Lifecycles](https://developer.android.com/topic/libraries/architecture/lifecycle.html).

https://codelabs.developers.google.com/codelabs/android-lifecycles/#1

https://mp.weixin.qq.com/s/gQhBeKA2vGAkh3Tbqn0tEA

###### ViewModel

https://noteforme.github.io/2020/04/06/viewmodel/

https://www.jianshu.com/p/731ca42823ee

viewmodelstoreowner

###### navigation

https://developer.android.com/guide/navigation/

https://developer.android.com/guide/navigation/navigation-getting-started

https://codelabs.developers.google.com/codelabs/android-navigation/index.html?index=..%2F..%2Findex#0

Codelab 抽屉图标不显示

- BottomNavigationView 导航栏优化 recarete fragment
  
  https://medium.com/@freedom.chuks7/how-to-use-jet-pack-components-bottomnavigationview-with-navigation-ui-19fb120e3fb9

**NOTICE** : NavigationView each item ID is matching with fragment ID in navigation

https://jiangjiwei.site/post/navigation-zhi-fragment-qie-huan/

https://github.com/lwj1994/navigation-keep-state-fragment

https://github.com/STAR-ZERO/navigation-keep-fragment-sample

Bottom tab定位

https://stackoverflow.com/questions/50577356/android-jetpack-navigation-bottomnavigationview-with-youtube-or-instagram-like

全局共享

[googlesamples](https://github.com/googlesamples/android-architecture-components)

##### previously architecture

MVC MVP. MVVM三种架构介绍

http://www.jcodecraeer.com/a/anzhuokaifa/2017/1024/8636.html

https://mp.weixin.qq.com/s/Kc1826MQ3ReMkoIWlsQGVw

https://juejin.im/post/5dafc49b6fb9a04e17209922

https://juejin.im/post/5dafc49b6fb9a04e17209922#heading-13

- MVP

https://juejin.im/entry/5955e7166fb9a06bc23a8598

https://github.com/yaozs/YzsBaseActivity

多个activity对p的复用

https://juejin.im/post/599ce8016fb9a0247e4255f4

- Component

https://mp.weixin.qq.com/s/8_8gGpkpO2QFNkWgSRBwIg

https://www.jianshu.com/p/6a50ef1ef45c

https://developer.android.com/studio/build/dependencies#duplicate_classes

https://developer.android.com/studio/build/manifest-merge?hl=zh-cn

merge androidmanifest 合并

<https://developer.android.com/studio/build/manifest-merge

https://www.jianshu.com/p/14a822e42151

compose

https://mp.weixin.qq.com/s/0mAbKEuBH5HHYa23EcWalg

lifecycle

https://juejin.im/post/6847902220755992589

组件化 服务

https://juejin.im/post/6884492604370026503