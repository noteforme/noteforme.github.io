---
title: animation
comments: true
date: 2017-08-16 11:22:57
tags:
categories: ANDROID

---
Android动画,直接copy了
>The Android framework provides two animation systems: property animation and view animation. Both animation systems are viable options, but the property animation system, in general, is the preferred method to use, because it is more flexible and offers more features. In addition to these two systems, you can utilize Drawable animation, which allows you to load drawable resources and display them one frame after another.

可以看出官方更推崇 Property Animiation,更灵活而且提供更多的功能,看下３种动画方式
https://developer.android.com/guide/topics/graphics/overview.html
http://blog.csdn.net/yanbober/article/details/46481171


- Drawable Animation (Frame动画，帧动画)
  >Drawable animation involves displaying Drawable resources one after another, like a roll of film. This method of animation is useful if you want to animate things that are easier to represent with Drawable resources, such as a progression of bitmaps.

  通过人视觉的延迟，把图片一张连接一张，类似放电影

- View Animation(Tween动画)
>View Animation is the older system and can only be used for Views. It is relatively easy to setup and offers enough capabilities to meet many application's needs.

- Property Animation
 > Introduced in Android 3.0 (API level 11), the property animation system lets you animate properties of any object, including ones that are not rendered to the screen. The system is extensible and lets you animate properties of custom types as well.

# View Animation
第一种就pass了，看下View Animation:
>You can use the view animation system to perform tweened animation on Views. Tween animation calculates the animation with information such as the start point, end point, size, rotation, and other common aspects of an animation.

这种动画可以在一个视图容器执行　位置、大小、旋转、透明度的变化，使用XML定义更具可读性

