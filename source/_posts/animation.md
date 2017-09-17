---
title: animation
comments: true
date: 2017-08-16 11:22:57
tags:
categories: ANDROID

---
Android动画简介,直接copy了
>The Android framework provides two animation systems: property animation and view animation. Both animation systems are viable options, but the property animation system, in general, is the preferred method to use, because it is more flexible and offers more features. In addition to these two systems, you can utilize Drawable animation, which allows you to load drawable resources and display them one frame after another.

可以看出官方更推崇 Property Animiation,更灵活而且提供更多的功能,看下３种动画方式
https://developer.android.com/guide/topics/graphics/overview.html


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

>-set
    |-android:interpolator -> 插值器，影响动画的速度
        |-默认值 -> @android:anim/accelerate_decelerate_interpolator
    |-android:shareInterpolator -> 集合所有动画是否使用同一插值器
    |-android:fillAfter -> 动画结束后View是否停留在结束的位置
    |-android:startOffset -> 动画多少秒之后执行
    |-android:repeatMode -> 重复的模式,默认为restart,即重头开始重新运行,reverse即从结束开始向前重新运行
-TranslateAnimation -> 移动View
    |-<translate>
        |-android:fillAfter ->
        |-android:duration -> 表示动画持续的时间
        |-android:fromXDelta -> 表示 x 的起始值
        |-android:toXDelta -> 表示 x 的结束值
        |-android:fromYDelta -> 表示 y 的起始值
        |-android:toYDelta -> 表示 y 的结束值
-scaleAnimation -> 放大或者缩小View
    |-<scale>
        |-android:duration -> 表示动画持续的时间
        |-android:fromXScale -> 表示水平方向缩放的起始值
        |-android:fromYScale -> 表示竖直方向缩放的起始值
        |-android:pivotX -> 表示缩放中心点的 X 坐标
        |-android:pivotY -> 表示缩放中心点的 Y 坐标
        |-android:toXScale -> 表示水平方向缩放的结束值
        |-android:toYScale -> 表示竖直方向缩放的结束值
-RotateAnimation -> 旋转View
    |-<rotate>
        |-android:duration -> 表示动画持续的时间
        |-android:fromDegrees -> 旋转开始的角度
        |-android:toDegrees -> 旋转结束的角度
        |-android:pivotX -> 旋转中心点的 X 坐标
        |-android:pivotY -> 旋转中心点的 Y 坐标
-AlphaAnimation -> 改变View的透明度
    |-<alpha>
        |-android:duration -> 表示动画持续的时间
        |-android:fromAlpha -> 透明度的起始值
        |-android:toAlpha -> 透明度的结束值
-自定义View动画 -> (不会,待实践中学习)


view动画示例:　文件位置放哪呢？　在官网确认了了位置

>The animation XML file belongs in the **res/anim/** directory of your Android project. The file must have a single root element: this will be either a single <alpha>, <scale>, <translate>, <rotate>, interpolator element, or <set> element that holds groups of these elements (which may include another <set>). By default, all animation instructions are applied simultaneously. To make them occur sequentially, you must specify the startOffset attribute, as shown in the example below.

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="1000"
    android:fillAfter="true"
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"
    android:shareInterpolator="true">
    <!-- 位置移动-->
    <translate
        android:fromXDelta="0.0"
        android:fromYDelta="0.0"
        android:toXDelta="500.0"
        android:toYDelta="500.0" />
    <!-- 放大或缩小 -->
    <scale
        android:fromXScale="0.0"
        android:fromYScale="0.0"
        android:pivotX="50%"
        android:pivotY="50%"
        android:toXScale="1.0"
        android:toYScale="1.0" />

    <rotate
        android:fromDegrees="0.0"
        android:pivotX="50%"
        android:pivotY="50%"
        android:toDegrees="30.0" />
    <alpha
        android:fromAlpha="0.0"
        android:toAlpha="0.5" />
</set>
```

[演示代码:](https://github.com/BlogForMe/AndroidDemo/blob/master/lern/src/main/java/com/hyhy/lern/AnimationActivity.java)
参考：http://www.jianshu.com/p/b7aa2a4a9787
http://blog.csdn.net/yanbober/article/details/46481171
 跑完代码后，点击动画结束后的图片，没反应，实际上按钮还是停留在屏幕左上角，只不过view动画把图片绘制到了新的位置。所以说　View动画是对View的影像做操作，它并不能改变View的位置参数
# Property Animation
　属性动画就是对　对象的属性进行实质的改变了，而不只是投影了，比如刚才的图片修改后是可以点击的
- 　ValueAnimator: 通过不断的对值进行操作来实现的，初始值盒结束值之间的动画过渡就是由ValueAnimator来负责计算的，除此之外，还负责管理动画的播放次数，播放模式，以及监听等。

        /**
         * 300 ms内,value值从0 平滑的过渡到了1
         */
//        ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);
//        anim.setDuration(300);

        /**
         *  value值从0过渡到５，再到３最后到10的过程
         */
//        ValueAnimator anim = ValueAnimator.ofFloat(0f, 5f, 3f, 10f);
        //整形数从 0到 100
        ValueAnimator anim = ValueAnimator.ofInt(0, 100);
        anim.setDuration(5000);
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                Object currentValue = valueAnimator.getAnimatedValue();
                Log.d(TAG, " current value is " + currentValue);
            }
        });
        anim.start();

- ObjectAnimator
ObjectAnimator继承ValueAnimator，它可以直接对任意对象的任意属性进行动画操作
演示示例
　　
        /**
         * 透明:　５秒内从常规变换成全　透明,再从透明变成常规
         */
//        ObjectAnimator animator = ObjectAnimator.ofFloat(v, "alpha", 1f, 0f, 1f);
        /**
         *  旋转 : 对按钮进行旋转 360
         */
//        ObjectAnimator animator = ObjectAnimator.ofFloat(v, "rotation", 0f, 360f);

        /**
         * 位置移动: Button向左移动再回来
         */
//        float curTranslationX = v.getTranslationX(); //获取当前Button的translationX位置
//        ObjectAnimator animator = ObjectAnimator.ofFloat(v, "translationX",
//                curTranslationX, -500f, curTranslationX); //传入translationX，后面３个参数告诉它怎么移动
        /**
         * 缩放： 参数改为 scaleY,表示再垂直方向上放大３倍再还原
         */
        ObjectAnimator animator = ObjectAnimator.ofFloat(v, "scaleY", 1f, 3f, 1f);
        animator.setDuration(5000);
        animator.start();
ObjectAnimator传入参数是根据Ｖiew的属性进行操作的，如setRotation()、getRotation()、setTranslationX()、getTranslationX()、setScaleY()、getScaleY()

- - -
＃ 组合动画 : 就是属性动画组合起来,借助AnimatorSet
　　微风吹着，飘来周传雄的音乐... 敲着键盘　人生不过如此～～
  
- after(Animator anim) : 现有动画放在传入动画之后执行
- after(long　delay)   : 將现有动画延迟　"delay" 后执行
- before(Animator anim) :参考楼上
- with(Animator anim)  :　现有动画盒传入动画同时执行  
　
 示例
 　　
    　　
      ObjectAnimator moveIn = ObjectAnimator.ofFloat(v, "translationX", -500f, 0f);
        ObjectAnimator rotation = ObjectAnimator.ofFloat(v, "rotation", 0f, 360f);
        ObjectAnimator fadeInOut = ObjectAnimator.ofFloat(v, "alpha", 1f, 0f, 1f);
        AnimatorSet animSet = new AnimatorSet();
        animSet.play(rotation).with(fadeInOut).after(moveIn);
        animSet.setDuration(5000);
        animSet.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animation) {
                Log.d(TAG, "  onAnimationStart ");
            }

            @Override
            public void onAnimationEnd(Animator animation) {
                Log.d(TAG, "  onAnimationEnd ");
            }

            @Override
            public void onAnimationCancel(Animator animation) {
                Log.d(TAG, "  onAnimationCancel ");
            }

            @Override
            public void onAnimationRepeat(Animator animation) {
                Log.d(TAG, "  onAnimationRepeat ");
            }
        });
        animSet.start();
　　下面是监听器，也可以用AnimatorListenerAdapter 选择想要重写的方法
# XML支持的Property Animation　tags
- 对应代码中的ValueAnimator - <animator>
- ObjectAnimator -  <objectAnimator>
- AnimatorSet   - <set>

这里也需要建个文件夹存放
>The property animation system lets you declare property animations with XML instead of doing it programmatically. By defining your animations in XML, you can easily reuse your animations in multiple activities and more easily edit the animation sequence.

>To distinguish animation files that use the new property animation APIs from those that use the legacy view animation framework, starting with Android 3.1, you should save the XML files for property animations in the **res/animator/ **directory.

```
<?xml version="1.0" encoding="utf-8"?>
<animator xmlns:android="http://schemas.android.com/apk/res/android"
    android:valueFrom="0"
    android:valueTo="100"
    android:valueType="intType" />

加载
Animator anim = AnimatorInflater.loadAnimator(this, R.animator.propery_first);
        anim.setTarget(v);
　       anim.start();

```
根据官方例子,也可以强转ValueAnimator，添加监听器,其他形式的xml文件也类似



参考：http://blog.csdn.net/guolin_blog/article/details/43536355