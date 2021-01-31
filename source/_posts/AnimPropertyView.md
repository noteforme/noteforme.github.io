---
title: AnimPropertyView
comments: true
date: 2019-12-29 22:23:37
tags:
categories: anim
---

 

#### 估值器（TypeEvaluator）

设置动画 如何从初始值 过渡到 结束值 的逻辑

* 插值器（`Interpolator`）决定 值 的变化模式（匀速、加速）
* 估值器（`TypeEvaluator`）决定 值 的具体变化数值



####  ValueAnimator

实现原理 : 通过不断控制 值的变化， 再不断赋值给对象的属性，从而实现动画的效果。

<img src="/Users/john/Documents/noteforme.github.io/source/_posts/AnimPropertyView/Screen Shot 2021-01-31 at 10.02.48 PM.png" style="zoom:80%;" />

从上面原理可以看出：`ValueAnimator`类中有3个重要方法：

1. `ValueAnimator.ofInt（int values）`
2. `ValueAnimator.ofFloat（float values）`
3. `ValueAnimator.ofObject（int values）`



#### ValueAnimator.ofInt（int values）

将初始值 **以整型数值的形式** 过渡到结束值

> 即估值器是整型估值器 - `IntEvaluator`



##### 模板代码

实际开发中，建议使用Java代码实现属性动画：因为很多时候属性的起始值是无法提前确定的（无法使用XML设置），这就需要在`Java`代码里动态获取。



```java
// 步骤1：设置动画属性的初始值 & 结束值
ValueAnimator anim = ValueAnimator.ofInt(0, 3);
        // ofInt（）作用有两个
        // 1. 创建动画实例
        // 2. 将传入的多个Int参数进行平滑过渡:此处传入0和1,表示将值从0平滑过渡到1
        // 如果传入了3个Int参数 a,b,c ,则是先从a平滑过渡到b,再从b平滑过渡到C，以此类推
        // ValueAnimator.ofInt()内置了整型估值器,直接采用默认的.不需要设置，即默认设置了如何从初始值 过渡到 结束值
        // 关于自定义插值器我将在下节进行讲解
        // 下面看看ofInt()的源码分析 ->>关注1
        
// 步骤2：设置动画的播放各种属性
        anim.setDuration(500);
        // 设置动画运行的时长
        
        anim.setStartDelay(500);
        // 设置动画延迟播放时间

        anim.setRepeatCount(0);
        // 设置动画重复播放次数 = 重放次数+1
        // 动画播放次数 = infinite时,动画无限重复
        
        anim.setRepeatMode(ValueAnimator.RESTART);
        // 设置重复播放动画模式
        // ValueAnimator.RESTART(默认):正序重放
        // ValueAnimator.REVERSE:倒序回放
     
// 步骤3：将改变的值手动赋值给对象的属性值：通过动画的更新监听器
        // 设置 值的更新监听器
        // 即：值每次改变、变化一次,该方法就会被调用一次
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {

                int currentValue = (Integer) animation.getAnimatedValue();
                // 获得改变后的值
                
                System.out.println(currentValue);
                // 输出改变后的值

        // 步骤4：将改变后的值赋给对象的属性值，下面会详细说明
                View.setproperty（currentValue）；

       // 步骤5：刷新视图，即重新绘制，从而实现动画效果
                View.requestLayout();
                
                
            }
        });

        anim.start();
        // 启动动画
    }

// 关注1：ofInt（）源码分析
    public static ValueAnimator ofInt(int... values) {
        // 允许传入一个或多个Int参数
        // 1. 输入一个的情况（如a）：从0过渡到a；
        // 2. 输入多个的情况（如a，b，c）：先从a平滑过渡到b，再从b平滑过渡到C
        
        ValueAnimator anim = new ValueAnimator();
        // 创建动画对象
        anim.setIntValues(values);
        // 将传入的值赋值给动画对象
        return anim;
    }
```



##### XML设置

```xml
// ValueAnimator采用<animator>  标签
<animator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="0"   // 初始值
    android:valueTo="100"  // 结束值
    android:valueType="intType" // 变化值类型 ：floatType & intType

    android:duration="3000" // 动画持续时间（ms），必须设置，动画才有效果
    android:startOffset ="1000" // 动画延迟开始时间（ms）
    android:fillBefore = “true” // 动画播放完后，视图是否会停留在动画开始的状态，默认为true
    android:fillAfter = “false” // 动画播放完后，视图是否会停留在动画结束的状态，优先于fillBefore值，默认为false
    android:fillEnabled= “true” // 是否应用fillBefore值，对fillAfter值无影响，默认为true
    android:repeatMode= “restart” // 选择重复播放动画模式，restart代表正序重放，reverse代表倒序回放，默认为restart|
    android:repeatCount = “0” // 重放次数（所以动画的播放次数=重放次数+1），为infinite时无限重复
    android:interpolator = @[package:]anim/interpolator_resource // 插值器，即影响动画的播放速度,下面会详细讲
/>  
```



```java
Animator animator = AnimatorInflater.loadAnimator(context, R.animator.set_animation);  
// 载入XML动画

animator.setTarget(view);  
// 设置动画对象

animator.start();  
// 启动动画
```



##### 实战 

按钮的宽度从 `200px` 放大到 `500px`

```xml
<Button
    android:id="@+id/bt_value_anim"
    android:layout_width="200px"
    android:layout_height="wrap_content"
    android:layout_centerInParent="true"
    android:text="动画" />
```

？代码设置初始值

```kotlin
// 步骤1：设置属性数值的初始值(此处在xml中200) & 结束值(600)
val valueAnimator = ValueAnimator.ofInt(bt_value_anim.layoutParams.width, 600)
    // ValueAnimator.ofInt()内置了整型估值器,直接采用默认的.不需要设置
       // 即默认设置了如何从初始值200 过渡到 结束值500

// 步骤2：设置动画的播放各种属性
valueAnimator.duration = 2000

// 步骤3：将改变的值手动赋值给对象的属性值：通过动画的更新监听器
        // 设置 值的更新监听器
        // 即：值每次改变、变化一次,该方法就会被调用一次
valueAnimator.addUpdateListener {
    val currentValue = it.animatedValue
    // 获得每次变化后的属性值
    Timber.d("currentValue $currentValue")

    // 每次值变化时，将值手动赋值给对象的属性
    bt_value_anim.layoutParams.width = currentValue as Int
    // 即将每次变化后的值 赋 给按钮的宽度，这样就实现了按钮宽度属性的动态变化

  // 步骤4：刷新视图，即重新绘制，从而实现动画效果
    bt_value_anim.requestLayout()
}

valueAnimator.start()
```



#### 浮点型：ValueAnimator.oFloat（）

将初始值 以浮点型数值的形式 过渡到结束值

- `ValueAnimator.oFloat（）`采用默认的浮点型估值器 (`FloatEvaluator`)
- `ValueAnimator.ofInt（）`采用默认的整型估值器（`IntEvaluator`）

在使用上完全没有区别



##### ValueAnimator.ofObject（） 

将初始值 以对象的形式 过渡到结束值

> 即通过操作 对象 实现动画效果





#### `ObjectAnimator`与 `ValueAnimator`类的区别

* `ValueAnimator` 类是先改变值，然后 **手动赋值** 给对象的属性从而实现动画；是 **间接** 对对象属性进行操作；
* `ObjectAnimator` 类是先改变值，然后 **自动赋值** 给对象的属性从而实现动画；是 **直接** 对对象属性进行操作

###  估值器（TypeEvaluator) 和插值器

1. 插值器（`Interpolator`）决定 值 的变化模式（匀速、加速）
2. 估值器（`TypeEvaluator`）决定 值 的具体变化数值







https://blog.csdn.net/carson_ho/article/details/99619871

作者：Carson_Ho
链接：https://www.jianshu.com/p/7c95342f4bc2
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。