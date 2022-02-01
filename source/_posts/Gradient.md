---
title: Gradient
comments: true
date: 2019-07-22 17:41:27
tags:
categories: VIEW
---

##### LinearGradient

```
/**
 * LinearGradient
 * Create a shader that draws a linear gradient along a line.
 *
 * @param x0           The x-coordinate for the start of the gradient line
 * @param y0           The y-coordinate for the start of the gradient line
 * @param x1           The x-coordinate for the end of the gradient line
 * @param y1           The y-coordinate for the end of the gradient line
 * @param colors       The colors to be distributed along the gradient line
 * @param positions    May be null. The relative positions [0..1] of
 *                     each corresponding color in the colors array. If this is null,
 *                     the the colors are distributed evenly along the gradient line.
 					 所占比例，渲染颜色所占比例,如果传null，则均匀渲染
 * @param tile         The Shader tiling mode
 */
val colors = intArrayOf(
    Color.RED, Color.GREEN, Color.BLUE
)
val positions = floatArrayOf(0.5f, 1f, 0.5f)
val linearGradient = LinearGradient(0f, 300f, 300f, 300f, colors, positions, Shader.TileMode.CLAMP)
val mPaint = Paint()
mPaint.setShader(linearGradient)
canvas?.drawRect(0f, 0f, 300f, 300f, mPaint)
```

* LinearGradient（0f, 0f, 300f, 300f）,说明从 (0,0) 到(300,300)对角线方向渲染

  ![LinearGradient](Gradient\gradient_20171214100743840.png)

<https://www.jianshu.com/p/5fb82b189094>

* 渐变模式 

  -CLAMP
  边缘拉伸
  -REPEAT
  在水平和垂直两个方向上重复，相邻图像没有间隙
  -MIRROR

  以镜像的方式在水平和垂直两个方向上重复，相邻图像有间隙

     