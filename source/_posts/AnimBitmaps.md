---
title: AnimBitmaps
comments: true
date: 2019-12-25 15:38:00
tags:
categories: anim
---

#####  Overview

https://developer.android.com/training/animation

# Animate drawable graphics

The first option is to use an [Animation Drawable](https://developer.android.com/guide/topics/graphics/drawable-animation.html#AnimationDrawable). This allows you to specify several static [drawable files](https://developer.android.com/guide/topics/graphics/2d-graphics.html) that will be displayed one at a time to create an animation. The second option is to use an [Animated Vector Drawable](https://developer.android.com/guide/topics/graphics/drawable-animation.html#AnimVector), which lets you animate the properties of a [vector drawable](https://developer.android.com/guide/topics/graphics/vector-drawable-resources.html).

1. The XML file consists of `<animation=list>`

   ```xml
   <animation-list xmlns:android="http://schemas.android.com/apk/res/android"
       android:oneshot="true">
       <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
       <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
       <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
   </animation-list>
   ```

2.   in which the animation is added to an `ImageView` and then animated when the  screen is touched

   ```
   private lateinit var rocketAnimation: AnimationDrawable
   
   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       setContentView(R.layout.main)
   
       val rocketImage = findViewById<ImageView>(R.id.rocket_image).apply {
           setBackgroundResource(R.drawable.rocket_thrust)
           rocketAnimation = background as AnimationDrawable
       }
   
       rocketImage.setOnClickListener({ rocketAnimation.start() })
   }
   ```

It's important to note that the `start()` method called on the `AnimationDrawable` cannot be called during the `onCreate()` method of your Activity, because the `AnimationDrawable` is not yet fully attached to the window. If you want to play the animation immediately, without requiring interaction, then you might want to call it from the `onStart()` method in your Activity, which will get called when Android makes the view visible on screen.

##### AnimatedVectorDrawable

