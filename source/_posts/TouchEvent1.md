今天学习Android的是事件分发机制的部分内容。

Android事件分发机制是指在Android系统中，如何将触摸事件（TouchEvent）从系统分发到各个View组件。这个过程主要涉及三个方法：`dispatchTouchEvent`、`onInterceptTouchEvent`、`onTouchEvent`。下面是对这三个方法及其工作流程的详细介绍：

### 1. `dispatchTouchEvent`

- **定义**：所有的ViewGroup和View都会重写这个方法。它是整个事件分发链的起点。
- **作用**：决定是否将事件传递给子View，如果子View没有处理，则调用自己的`onTouchEvent`方法进行处理。

### 2. `onInterceptTouchEvent`

- **定义**：只有ViewGroup有这个方法。普通的View是没有的。
- **作用**：用于决定是否拦截子View的事件。如果返回true，表示拦截事件，不再向子View传递，而是由当前ViewGroup处理。

### 3. `onTouchEvent`

- **定义**：所有的View和ViewGroup都有这个方法。
- **作用**：最终处理触摸事件的地方。可以在这里定义具体的触摸事件响应逻辑，如点击、长按、滑动等。

### 事件分发流程

1. **Activity级别**：
   
   - `Activity.dispatchTouchEvent()`接收到事件。
   - 如果返回true，表示事件被处理，停止向下传递。
   - 如果返回false，事件继续传递给当前窗口的顶级View。

2. **ViewGroup级别**：
   
   - `ViewGroup.dispatchTouchEvent()`接收到事件。
   - 调用`onInterceptTouchEvent()`来判断是否拦截事件。
     - 如果`onInterceptTouchEvent()`返回true，则调用自身的`onTouchEvent()`进行处理。
     - 如果返回false，则将事件传递给子View的`dispatchTouchEvent()`。

3. **View级别**：
   
   - `View.dispatchTouchEvent()`接收到事件。
   - 调用自身的`onTouchEvent()`进行处理。

### 示例代码

以下是一个简单的示例，展示了事件分发机制的基本工作原理：

```
public class CustomViewGroup extends ViewGroup {

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        Log.d("CustomViewGroup", "dispatchTouchEvent: " + ev.getAction());
        return super.dispatchTouchEvent(ev);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        Log.d("CustomViewGroup", "onInterceptTouchEvent: " + ev.getAction());
        return true; // 拦截所有事件
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        Log.d("CustomViewGroup", "onTouchEvent: " + ev.getAction());
        return true; // 处理事件
    }

    // 省略布局相关代码...
}

public class CustomView extends View {

    public CustomView(Context context) {
        super(context);
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        Log.d("CustomView", "dispatchTouchEvent: " + ev.getAction());
        return super.dispatchTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        Log.d("CustomView", "onTouchEvent: " + ev.getAction());
        return true; // 处理事件
    }
}
```

项目中guideview用到了

ShouldDelayChildPressedState

这个你也看一下是否要加 系统级别的延迟,别看了 你继承的class 不会延迟

<img src="TouchEvent1/ShouldDelayChildPressedState.png" alt="ShouldDelayChildPressedState" style="zoom: 50%;" />

#### Question

手机屏幕是由一个个像素点渲染而成的，不同的布局类似于幕布一层层叠上去形成了屏幕上显示的效果，The action is from  DOWN、MOVE、UP, How they formed while we used like longClick , touch, click?

a button on the listview item，why the listview item don't response while we click the button in a interview ?  but I cann't answer the key point.

multible listview or recycleview in a  Scrollview  ,how to resolve it?

Android provide NestedScrollView for us now,how does it work?

#### Answer

##### Image  clear

<img title="" src="TouchEvent1/e212186f23a1ff62130f8cc625ebd5fdfd9a848f.png" alt="touch" style="zoom: 67%;">

I found it very useful when i looked it.it make me have global view.and then I read Activity ViewGroup and View source code from framework. But I found some place could do better ,so I draw another Image.

<img src="TouchEvent1/ACTION_DOWN No childView.png" alt="action down" style="zoom:67%;" />

<img src="TouchEvent1/ACTION_DOWN.png" alt="down" style="zoom:67%;" />

<img src="TouchEvent1/ACTION_MOVE UP.png" alt="move" style="zoom:67%;" />

开发中常用的事件分发

ListView　

item 上有Button ,item上的onItemClick事件得不到响应，可以看看官方[ViewGroup](https://developer.android.com/reference/android/view/ViewGroup.html#attr_android:descendantFocusability) 描述

##### item Button

> 页面搜索　android:descendantFocusability
> 
> android:descendantFocusability属性共有三个取值，分别为
> beforeDescendants：viewgroup会优先其子类控件而获取到焦点
> afterDescendants：viewgroup 只有当其子类控件不需要获取焦点时才获取焦点
> blocksDescendants：viewgroup 会覆盖子类控件而直接获得焦点

原因 : 在View `onTouchEvent(MotionEvent event) `中,只要view可点击就返回true,事件就被消费掉了,
    set isClickable = true  才可以收到 action move up事件

```java
public boolean onTouchEvent(MotionEvent event) {
    final float x = event.getX();
    final float y = event.getY();
    final int viewFlags = mViewFlags;
    final int action = event.getAction();

    final boolean clickable = ((viewFlags & CLICKABLE) == CLICKABLE
            || (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)
            || (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE;

    if ((viewFlags & ENABLED_MASK) == DISABLED) {
        if (action == MotionEvent.ACTION_UP && (mPrivateFlags & PFLAG_PRESSED) != 0) {
            setPressed(false);
        }
        mPrivateFlags3 &= ~PFLAG3_FINGER_DOWN;
        // A disabled view that is clickable still consumes the touch
        // events, it just doesn't respond to them.
        return clickable;
    }
 }
```

按钮是可以点击的直接被onTouchEvent消费
