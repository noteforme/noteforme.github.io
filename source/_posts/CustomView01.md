---

title: CustomView01
comments: true
date: 2017-11-12 21:24:33
tags: VIEW
categories: VIEW
---

##### 自定义View函数调用流程 

 ![流程](CustomView01\2019-04-29-71649.jpg)

> 出自:<http://gcssloop.github.io/customview/CustomViewProcess>



* View OnMeasure()方法

  ```
  val widthMode = MeasureSpec.getMode(widthMeasureSpec);
  val widthSize = MeasureSpec.getSize(widthMeasureSpec); widthSize父控件留个子空间的最大宽度
  
  val heightMode = MeasureSpec.getMode(heightMeasureSpec);
  val heightSize = MeasureSpec.getSize(heightMeasureSpec);
  
  var width = 10
  var height = 10
  if (widthMode == MeasureSpec.EXACTLY) {   //如果match_parent或者具体的值，直接赋值
      width = widthSize
  } else if (widthMode == MeasureSpec.AT_MOST) { //如果是wrap_content，我们要得到控件需要多大的尺寸
      width = paddingLeft + mBound.width() + paddingRight
  }
  //高度跟宽度处理方式一样
  if (heightMode == MeasureSpec.EXACTLY) {
      height = heightSize
  } else if (heightMode == MeasureSpec.AT_MOST) {
      height = paddingTop + mBound.height() + paddingBottom
  }
  setMeasuredDimension(width, height)
  ```

理解onMeasure <https://blog.csdn.net/tuke_tuke/article/details/73302595>

* 加载布局

![图](CustomView01\4131410-a0f7a6a89a5509ac.webp)





##### Paint

为了展示方便，容易看出效果，之前使用的模式一直为填充模式，实际上画笔有三种模式，如下：

```
STROKE                //描边
FILL                  //填充
FILL_AND_STROKE       //描边加填充
```

为了区分三者效果我们做如下实验：

```
        Paint paint = new Paint();
        paint.setColor(Color.BLUE);
        paint.setStrokeWidth(40);     //为了实验效果明显，特地设置描边宽度非常大

        // 描边
        paint.setStyle(Paint.Style.STROKE);
        canvas.drawCircle(200,200,100,paint);

        // 填充
        paint.setStyle(Paint.Style.FILL);
        canvas.drawCircle(200,500,100,paint);

        // 描边加填充
        paint.setStyle(Paint.Style.FILL_AND_STROKE);
        canvas.drawCircle(200, 800, 100, paint);
```

<img src="https://camo.githubusercontent.com/cb4ff7eb6df1e98d082a48ceaf4c29370e236103/687474703a2f2f7777342e73696e61696d672e636e2f6c617267652f30303558746469326a77316632373467366c7862706a3330753031686371336e2e6a7067" alt="显示图" style="zoom:25%;" />



##### 画布

坐标操作

```
        canvas.scale(1,-1);                 // 翻转Y轴
```





##### 自定义属性类别

> boolean     表示attr取值为true或者false
> color         表示attr取值是颜色类型，例如#ff3344,或者是一个指向color的资源id，例如R.color.colorAccent.
> dimension 表示 attr 取值是尺寸类型，例如例如取值16sp、16dp，也可以是一个指向dimen的资源id，例		  如R.dimen.dp_16
> float 	 表示attr取值是整形或者浮点型
> fraction     表示 attr取值是百分数类型，只能以%结尾，例如30%
> integer      表示attr取值是整型
> string        表示attr取值是String类型，或者一个指向String的资源id，例如R.string.testString
> reference   表示attr取值只能是一个指向资源的id。
> enum 	表示attr取值只能是枚举类型。



**refrence , 表示attr取值只能是一个指向资源的id。**

<attr name="color" format="refrence"/>

注意  dimension ,

```
   val a = context!!.obtainStyledAttributes(attrs, R.styleable.ViewBodySleep , defStyleAttr, 0)
        linePaint.textSize   = a.getDimensionPixelSize(R.styleable.ViewBodySleep_vbltextsize,21).toFloat()
        indexPaint.textSize   = a.getDimensionPixelSize(R.styleable.ViewBodySleep_vbltextsize,21).toFloat()

        a.recycle()
```







 https://github.com/GcsSloop/AndroidNote/tree/master/CustomView



https://www.sunzn.com/2017/08/21/%E8%87%AA%E5%AE%9A%E4%B9%89-View-%E5%9F%BA%E7%A1%80-%E8%A7%92%E5%BA%A6%E4%B8%8E%E5%BC%A7%E5%BA%A6/





##### setWillNotDraw

ViewGroup默认情况下，出于性能考虑，会被设置成WILL_NOT_DROW，这样，ondraw就不会被执行了。

如果我们想重写一个viewgroup的ondraw方法，有两种方法：

1，构造函数中，给viewgroup设置一个颜色。

2，构造函数中，调用setWillNotDraw（false），去掉其WILL_NOT_DRAW flag。

在viewgroup初始化的时候，它调用了一个私有方法：initViewGroup，它里面会有一句setFlags（WILLL_NOT_DRAW,DRAW_MASK）;相当于调用了setWillNotDraw（true），所以说，对于ViewGroup，他就认为是透明的了，如果我们想要重写onDraw，就要调用setWillNotDraw（false）。



https://www.jianshu.com/p/7df7e8a0b1a6

https://juejin.im/post/5e6e0b91f265da5716712288

RelativeLayout就是拓扑排序，用到了不光是树，还有图。所以数据结构与算法真的非常重要，他会让你的学习不会再那么枯燥。



childview

https://www.jianshu.com/p/c84693096e41