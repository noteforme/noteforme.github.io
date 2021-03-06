---

title: View_OVER
comments: true
date: 2017-11-12 21:24:33
tags: VIEW
categories: VIEW
---

自定义View函数调用流程 

 ![流程](View_OVER\2019-04-29-71649.jpg)

> 出自:<http://gcssloop.github.io/customview/CustomViewProcess>





#### 窗口显示



 ![流程](View_OVER\2021-02-21_at_5.19.41.png)



每个Activity包含一个Window对象，Android中window对象由PhoneWindow实现，PhoneWindow将一个DecorView设置为整个应用窗口的根View,DecorView作为窗口界面的顶层视图，封装了窗口操作的通用方法，DecorView将要显示的具体内容显示在PhoneWindow上，这里所有的View监听事件通过WindowMangerService来接收,通过Activity对象来回调相应的onCLicklistener.显示是将屏幕分成两部分，一个TitleView，另一个是ContentView.

#### Measure

如果是原始的 View,通过measure方法就完成了测量过程,如果是ViewGroup,除了完成自己的测量外，还需要遍历所有的子View,各个子元素再去递归执行这个流程.



##### View OnMeasure()方法

```
val widthMode = MeasureSpec.getMode(widthMeasureSpec);
val widthSize = MeasureSpec.getSize(widthMeasureSpec); widthSize父控件留个子空间的最大宽度

val heightMode = MeasureSpec.getMode(heightMeasureSpec);
val heightSize = MeasureSpec.getSize(heightMeasureSpec);

var width = 10
var height = 10
if (widthMode == MeasureSpec.EXACTLY) {   //如果match_parent或者具体的值，直接赋值
    width = widthSize
} else if (widthMode == MeasureSpec.AT_MOST) { //w，我们要得到控件需要多大的尺寸
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

加载布局

<img src="View_OVER\2021-02-16_at_3.37.39.png" alt="图" style="zoom:80%;" />

 

##### onSizeChanged

因为View的大小不仅由View本身控制，而且受父控件的影响，所以我们在确定View大小的时候最好使用系统提供的onSizeChanged回调函数。它又四个参数，分别为 宽度，高度，上一次宽度，上一次高度。这个函数比较简单，我们只需关注 宽度(w), 高度(h) 即可，这两个参数就是View最终的大小。

链接：https://juejin.cn/post/6844903448186454030


####  Layout

`public void layout(int l, int t, int r, int b) `

该控件的左 上 右 下边距



#### Scroller

##### scrollTo scrollBy

在 View 类中，有两个变量 `mScrollX` 和 `mScrollY`，它们记录的是 View 的内容的偏移值。`mScrollX` 和 `mScrollY` 的默认值都是 0，即默认不偏移。另外我们需要知道一点，向左滑动，`mScrollX` 为正数，反正为负数。假设我们令 `mScrollX = 10`，那么该 View 的内容会相对于原来向左偏移 10px。 看看系统的 View 类中的源码：

```java
public class View {

  /**
  * The offset, in pixels, by which the content of this view is scrolled
  * horizontally.
  * {@hide}
  */
  protected int mScrollX; //整个滑动过程的 X移动范围
  
  /**
  * The offset, in pixels, by which the content of this view is scrolled
  * vertically.
  * {@hide}
  */
  protected int mScrollY;
  
  
}
```



滑动绝对距离

```java
public void scrollTo(int x, int y)
```

滑动相对距离

```java
public void scrollBy(int x, int y)
```



##### Scroller辅助类

HorizontalScrollViewEx

```
public class HorizontalScrollViewEx extends ViewGroup {
    private static final String TAG = "HorizontalScrollViewEx";

    private int mChildrenSize;
    private int mChildWidth;
    private int mChildIndex;

    // 分别记录上次滑动的坐标
    private int mLastX = 0;
    private int mLastY = 0;
    // 分别记录上次滑动的坐标(onInterceptTouchEvent)
    private int mLastXIntercept = 0;
    private int mLastYIntercept = 0;

    private Scroller mScroller;
    private VelocityTracker mVelocityTracker;

    public HorizontalScrollViewEx(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }


    private void init() {
        mScroller = new Scroller(getContext());
        mVelocityTracker = VelocityTracker.obtain();
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        mVelocityTracker.addMovement(event);
        int x = (int) event.getX();
        int y = (int) event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: {
                if (!mScroller.isFinished()) {
                    mScroller.abortAnimation();
                }
                break;
            }
            case MotionEvent.ACTION_MOVE: {
                int deltaX = x - mLastX;
                int deltaY = y - mLastY;
                scrollBy(-deltaX, 0);
                break;
            }
            case MotionEvent.ACTION_UP: {
                int scrollX = getScrollX();
//                int scrollToChildIndex = scrollX / mChildWidth;
                mVelocityTracker.computeCurrentVelocity(1000);
                float xVelocity = mVelocityTracker.getXVelocity();
                if (Math.abs(xVelocity) >= 50) {
                    mChildIndex = xVelocity > 0 ? mChildIndex - 1 : mChildIndex + 1;
                } else {
                    mChildIndex = (scrollX + mChildWidth / 2) / mChildWidth;
                }

                mChildIndex = Math.max(0, Math.min(mChildIndex, mChildrenSize - 1));
                Log.d(TAG,  "xVelocity "+xVelocity+ " mChildIndex " +mChildIndex +" mChildWidth " + mChildWidth);

                int dx = mChildIndex * mChildWidth - scrollX;

                Log.d(TAG,  " scrollX " + scrollX + " dx " + dx);
                smoothScrollBy(dx, 0);
                mVelocityTracker.clear();
                break;
            }
            default:
                break;
        }
        mLastX = x;
        mLastY = y;
        return true;
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        Log.d(TAG, "left " + l + " top " + t + " right " + r + " bottom " + b);
        int childLeft = 0;
        final int childCount = getChildCount();
        mChildrenSize = childCount;
        for (int i = 0; i < childCount; i++) {
            final View childView = getChildAt(i);
            if (childView.getVisibility() != View.GONE) {
                final int childWidth = childView.getMeasuredWidth();
                mChildWidth = childWidth;
                childView.layout(childLeft, 0, childLeft + childWidth,
                        childView.getMeasuredHeight());
                childLeft += childWidth;
            }
        }
    }

    private void smoothScrollBy(int dx, int dy) {
        mScroller.startScroll(getScrollX(), 0, dx, 0, 500);
        invalidate();
    }

    @Override
    public void computeScroll() {
        if (mScroller.computeScrollOffset()) {
            Log.d(TAG,"getCurrX "+mScroller.getCurrX() + "mScroller.getCurrY() " +mScroller.getCurrY());
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            postInvalidate();
        }
    }

    @Override
    protected void onDetachedFromWindow() {
        mVelocityTracker.recycle();
        super.onDetachedFromWindow();
    }
}
```



可知View的scrollTo()、scrollBy()是瞬间完成的，当我们的手指在屏幕上移动时，内容会跟着手指滑动，但是当我们手指一抬起时，滑动就会停止,为使得滚动更加平滑,Scroller只是计算辅助类，它的startScroll()和computeScrollOffset()方法中也只是对一些轨迹参数进行设置和计算，真正需要进行滑动还是得通过View的scrollTo()、scrollBy()方法

```java
// 开始滚动，并记下当前时间点作为开始滚动的时间点
startX startY，滑动时起点偏移量，
public void startScroll(int startX, int startY, int dx, int dy, int duration) 
```

开始一个动画控制，由(startX , startY)在duration时间内前进(dx,dy)个单位，即到达偏移坐标为(startX+dx , startY+dy)处。



滑动日志,startX+dx是1080 2160,印证了上面的判断

```
com.ryg.chapter_3 D/HorizontalScrollViewEx:  scrollX 821 dx 259
com.ryg.chapter_3 D/HorizontalScrollViewEx:  scrollX 2007 dx 153
```



实际滑动处理类，Scroller类应该有个插值器，提供滑动控制

```
@Override
public void computeScroll() {
    if (mScroller.computeScrollOffset()) {
        Log.d(TAG,"getCurrX "+mScroller.getCurrX() + "mScroller.getCurrY() " +mScroller.getCurrY());
        scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
        postInvalidate();
    }
}
```



http://chanthuang.github.io/2016/08/31/Android-View-%E7%9A%84%E6%BB%9A%E5%8A%A8%E5%8E%9F%E7%90%86%E5%92%8C-Scroller%E3%80%81VelocityTracker-%E7%B1%BB%E7%9A%84%E4%BD%BF%E7%94%A8/

https://www.jianshu.com/p/f10809e1964b



#### Draw

Paint

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



https://mp.weixin.qq.com/s/psrDADxwl782Fbs_vzxnQg



##### UI 优化系列专题

https://www.jianshu.com/p/ea464eb15436



##### 淘宝详情

onLayout 两个子view切换绘制

https://juejin.cn/post/6844903828035207182