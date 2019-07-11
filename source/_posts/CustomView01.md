---
title: CustomView01
comments: true
date: 2017-11-12 21:24:33
tags: VIEW
categories: VIEW
---



#### Canvas基本操作

   这些操作都可以叠加



#### Rect RectF区别

 Rect是使用int类型作为数值，RectF是使用float类型作为数值。

四个参数是 矩形左上角和右下角两个点的坐标

#### 画线

```
       Path path = new Path();
       path.moveTo(100, 100);
       path.rLineTo(100, 200);
       canvas.drawPath(path, mPaint);
```

#### 画圆

- 画笔

  ```
  mPaint = new Paint();
  mPaint.setColor(Color.BLUE);
  mPaint.setStyle(Paint.Style.FILL); // Fill是空心圆 ，STROKE实心圆,
  空心圆就需要设置setStrokeWidth这个属性
  ```

- ```
  canvas.drawCircle(getWidth()/2,getHeight()/2,getWidth()/2,mPaint);
  ```

- ```
  public void drawArc(RectF oval, float startAngle, float sweepAngle, boolean useCenter, Paint paint)
  ```

  oval :指定圆弧的外轮廓矩形区域。
  startAngle: 圆弧起始角度，单位为度。
  sweepAngle: 圆弧扫过的角度(起始位置 X轴)，顺时针方向，单位为度,从右中间开始为零度。
  useCenter: 如果为True时，在绘制圆弧时将圆心包括在内，通常用来绘制扇形

RectF是矩形的内接圆





* 绘制形状文本

  ```
   canvas.drawPoint(200, 200, mPaint);
          canvas.drawPoints(new float[]{          //绘制一组点，坐标位置由float数组指定
                  500, 500,
                  500, 600,
                  500, 700
          }, mPaint);
  ```

* 位移(translate)

  ```
   // 在坐标原点绘制一个黑色圆形
          canvas.translate(200, 200);
          canvas.drawCircle(0, 0, 100, mPaint);
          // 在坐标原点绘制一个蓝色圆形
          mPaint.setColor(Color.BLUE);
          canvas.translate(200, 200);
          canvas.drawCircle(0, 0, 100, mPaint);
  ```

* 缩放(scale)

  ```
   canvas.translate(mWidth / 2, mHeight / 2);
          RectF rect = new RectF(0, -400, 400, 0);   // 矩形区域
          mPaint.setColor(Color.BLACK);           // 绘制黑色矩形
          canvas.drawRect(rect, mPaint);
          canvas.scale(-0.5f, -0.5f);          // 画布缩放  <-- 缩放中心向右偏移了200个单位
          mPaint.setColor(Color.BLUE);            // 绘制蓝色矩形
          canvas.drawRect(rect, mPaint);
          
          
          
          RectF rect = new RectF(-400, -400, 400, 400);
          canvas.drawRect(rect, mPaint);
          for (int i = 0; i < 20; i++) {
              canvas.scale(0.9f, 0.9f);
              canvas.drawRect(rect, mPaint);
          }
  ```

* 旋转(rotate)

  ```
  canvas.rotate(180);
  ```

  #####  drawPicture 三个方法

  > public void drawPicture (Picture picture) 
  >
  > public void drawPicture (Picture picture, Rect dst) 
  >
  > public void drawPicture (Picture picture, RectF dst)

  ##### drawBitmap 三个方法

  > // 第一种 public void drawBitmap (Bitmap bitmap, Matrix matrix, Paint paint) 
  >
  > // 第二种 public void drawBitmap (Bitmap bitmap, float left, float top, Paint paint)
  >
  >  // 第三种 public void drawBitmap (Bitmap bitmap, Rect src, Rect dst, Paint paint) public void drawBitmap (Bitmap bitmap, Rect src, RectF dst, Paint paint)

* 直接看第三种方法

```
        // 指定图片绘制区域(左上角的四分之一)
        Rect src = new Rect(0, 0, bitmap.getWidth() / 2, bitmap.getHeight() / 2);

        // 指定图片在屏幕上显示的区域
        Rect dst = new Rect(0, 0, 400, 500);
        //绘制图片
        canvas.drawBitmap(bitmap, src, dst, null);
```



参考：http://www.gcssloop.com/customview/Canvas_Convert





  https://www.jianshu.com/p/0bd672626c8d
