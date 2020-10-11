---
title: flutter_layout
comments: true
date: 2020-09-24 15:43:26
tags:
categories: flutter
---



https://jspang.com/detailed?id=43

https://book.flutterchina.club/chapter4/row_and_column.html



### 主轴和纵轴

对于线性布局，有主轴和纵轴之分，如果布局是沿水平方向(Row),那么主轴就是指水平方向，而纵轴即垂直方向.

如果布局沿垂直方向(Colomn)，那么主轴就是指垂直方向，而纵轴就是水平方向

* MainAxisAlignment

   表示子组件在`Row`所占用的水平空间内对齐方式，如果`mainAxisSize`值为`MainAxisSize.min`，则此属性无意义

* crossAxisAlignment 

  表示子组件在纵轴方向的对齐方式，`Row`的高度等于子组件中最高的子元素高度

* TextDirection : 字体方向

* CrossAxisAlignment : 表示`Row`纵轴（垂直）的对齐方向，默认是`VerticalDirection.down`，表示从上到下.

* **`Row`和`Column`都只会在主轴方向占用尽可能大的空间，而纵轴的长度则取决于他们最大子元素的长度**

  特殊情况 : 

  如果`Row`里面嵌套`Row`，或者`Column`里面再嵌套`Column`，那么只有最外面的`Row`或`Column`会占用尽可能大的空间，里面`Row`或`Column`所占用的空间为实际大小，下面以`Column`为例说明
  
  

