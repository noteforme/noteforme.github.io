---
title: RecyclerView-padding
date: 2023-01-10 15:16:04
tags: RecyclerView
categories: ANDROID
---



修改Display scaling属性后，即使设置字体是dp也会导致，也会出现Text内容的有变化。

计算转账页面的padding,5个TextView平分屏幕宽度，之前nexus5没问题，因为是刚好2倍，我的手机就是有问题，原来是dp转px有损耗。



```kotlin
@get:JvmName("dp2px")
val Float.dp: Float
    get() = convert(this, TypedValue.COMPLEX_UNIT_DIP)

@get:JvmName("dp2px")
val Int.dp: Int
    get() = convert(this.toFloat(), TypedValue.COMPLEX_UNIT_DIP).toInt()
```



然后用 2f.dp这样就能避免dp转px的损耗。



其实选择上面这个方法就能解决这个问题。



字体测量方法



```kotlin
fun getTextLength(typeface: Typeface, txt: String): Int {
    val paint = Paint()
    paint.typeface = typeface
    paint.textSize = 12f.dp.toFloat()
    val textWidth = paint.measureText(txt)
    return textWidth.toInt()
}
```



文字宽度

https://www.cnblogs.com/dasusu/p/6602710.html

https://www.jianshu.com/p/9fc07c3311c1

https://www.laoyuyu.me/2020/08/03/android/text_measure_wh/

