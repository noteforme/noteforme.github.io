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
  } else if (widthMode == MeasureSpec.AT_MOST) {
      height = paddingTop + mBound.height() + paddingBottom
  }
  setMeasuredDimension(width, height)
  ```

理解onMeasure <https://blog.csdn.net/tuke_tuke/article/details/73302595>

* 加载布局

![图](CustomView01\4131410-a0f7a6a89a5509ac.webp)





https://www.sunzn.com/2017/08/21/%E8%87%AA%E5%AE%9A%E4%B9%89-View-%E5%9F%BA%E7%A1%80-%E8%A7%92%E5%BA%A6%E4%B8%8E%E5%BC%A7%E5%BA%A6/