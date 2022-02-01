---

title: Architecture
comments: true
date: 2018-06-14 09:31:59
tags:
categories: Jetpack
---



#####  Jetpack



![](Architecture/2021-08-02_8.15_jetpack.png)

It helps to be familiar with software architectural patterns that separate data from the user interface, such as MVP or MVC

https://developer.android.com/jetpack/docs/getting-started

* modules should interact

![final-architecture](Architecture/final-architecture.png)



* NetworkBoundResource



It starts by observing the database for the resource. When the entry is loaded from the database for the first time, `NetworkBoundResource` checks whether the result is good enough to be dispatched or that it  should be re-fetched from the network. Note that both of these  situations can happen at the same time, given that you probably want to  show cached data while updating it from the network.

![final-architecture](Architecture/network-bound-resource.png)







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



* BottomNavigationView 导航栏优化  recarete fragment

  https://medium.com/@freedom.chuks7/how-to-use-jet-pack-components-bottomnavigationview-with-navigation-ui-19fb120e3fb9


**NOTICE** :  NavigationView each item ID is matching with fragment ID in navigation

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

* MVP

https://juejin.im/entry/5955e7166fb9a06bc23a8598

https://github.com/yaozs/YzsBaseActivity

多个activity对p的复用

https://juejin.im/post/599ce8016fb9a0247e4255f4



* Component

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



