---
title: view_custom_attr
date: 2022-08-27 21:53:08
tags:
categories: VIEW
---

##### 自定义属性类别

> boolean     表示attr取值为true或者false
> color         表示attr取值是颜色类型，例如#ff3344,或者是一个指向color的资源id，例如R.color.colorAccent.
> dimension 表示 attr 取值是尺寸类型，例如例如取值16sp、16dp，也可以是一个指向dimen的资源id，例          如R.dimen.dp_16
> float      表示attr取值是整形或者浮点型
> fraction     表示 attr取值是百分数类型，只能以%结尾，例如30%
> integer      表示attr取值是整型
> string        表示attr取值是String类型，或者一个指向String的资源id，例如R.string.testString
> reference   表示attr取值只能是一个指向资源的id。
> enum     表示attr取值只能是枚举类型。

CustomView.kt

```xml
<declare-styleable name="CustomView">
    <attr name="attr_boolean" format="boolean" />
    <attr name="attr_color" format="color" />
    <attr name="attr_dimension" format="dimension" />
    <attr name="attr_float" format="float" />
    <attr name="attr_fraction" format="fraction" />
    <attr name="attr_string" format="string" />
    <attr name="attr_reference" format="reference" />
    <attr name="attr_enum" format="enum" >
        <enum name="left" value="0"/>
        <enum name="right" value="1"/>
    </attr>
</declare-styleable>
```

```kotlin
val a = context.obtainStyledAttributes(attrs, R.styleable.CustomView, 0, defStyleAttr)
val boolean = a.getBoolean(R.styleable.CustomAttribute_customBoolean, false)
val color = a.getColor(R.styleable.CustomAttribute_customColorValue, R.color.colorPrimary)
val dimension = a.getDimension(R.styleable.CustomAttribute_customDimension, 20F)
val float = a.getFloat(R.styleable.CustomView_attr_float, 20f)
val fraction = a.getFraction(R.styleable.CustomView_attr_fraction, 1, 2, 0f)
val integer = a.getInteger(R.styleable.CustomAttribute_customIntegerValue, 0)
val string = a.getString(R.styleable.CustomView_attr_string)
resourceId =
    a.getResourceId(R.styleable.CustomAttribute_customReference, R.drawable.meitu13333)
var textPos = a.getInteger(R.styleable.CustomView_attr_enum, 0)
a.recycle()
```

**refrence , 表示attr取值只能是一个指向资源的id。**

 https://github.com/GcsSloop/AndroidNote/tree/master/CustomView

https://segmentfault.com/a/1190000012253911

##### onDraw  dispatchDraw

https://www.jianshu.com/p/89efaf8bd3dd

https://juejin.cn/post/6844903997917102093
