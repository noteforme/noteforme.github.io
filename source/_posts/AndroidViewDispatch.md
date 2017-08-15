---
title: AndroidViewDispatch
comments: true
date: 2017-08-13 22:01:04
tags:
categories: ANDROID

---
 ###Android事件分发机制
 事件分发其实就是对MotionEvent事件得分发过程,当MotionEvet产生后，系统需要把这个事件传递给一个具体得View,而这个传递得过程就是分发过程
事件分发机制事由　Activity -> ViewGroup -> View　方向上得传递
通过三个很重要得方法共同完成 dispatchTouchEvent、onInterceptTouchEvent、onTouchEvent

1.dispatchTouchEvent()
 属性   |   介绍
--------|--------
 使用对象 | Activity、ViewGroup、View
作用|分发点击事件

２.onTouchEvent()
   属性|介绍
 ------|------
 使用对象|Activity、ViewGroup、View
 作用 | 处理点击事件
调用时刻|在dispatchTouchEvent()内部调用

3、onInterceptTouchEvent()
  属性|介绍
-----|-----
作用 | 拦截事件，即自己处理该事件
调用时刻|在ViewGroup的dispatchTouchEvent()内部调用





##案例分析
# onTouch返回false
      button2.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                Log.d(TAG, "执行 button2 onTouch() 动作" + event.getAction());
                return false;
            }
        });

        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d(TAG, "you clicked button2");
            }
        });

打印结果:
 >D/DispatchActivity: 执行 button2 onTouch() 动作0
 D/DispatchActivity: 执行 button2 onTouch() 动作1
 D/DispatchActivity: you clicked button2

#onTouch返回true
Log打印结果:
> D/DispatchActivity: 执行 button2 onTouch() 动作0
 D/DispatchActivity: 执行 button2 onTouch() 动作1

不同得返回值,得到不同得结果　看下为什么

看下　View的dispatchTouchEvent（）　25得源码的事件分发流程

      /**
     * Pass the touch screen motion event down to the target view, or this
     * view if it is the target.
     *
     * @param event The motion event to be dispatched.
     * @return True if the event was handled by the view, false otherwise.
     */
    public boolean dispatchTouchEvent(MotionEvent event) {
       if (onFilterTouchEventForSecurity(event)) {
            if ((mViewFlags & ENABLED_MASK) == ENABLED && handleScrollBarDragging(event)) {
                result = true;
            }
            //noinspection SimplifiableIfStatement
            ListenerInfo li = mListenerInfo;
            if (li != null && li.mOnTouchListener != null
                    && (mViewFlags & ENABLED_MASK) == ENABLED
                    && li.mOnTouchListener.onTouch(this, event)) {
                result = true;
            }

            if (!result && onTouchEvent(event)) {
                result = true;
            }
        }
    }

从代码可以看到 要执行　＆＆ 右边的onTouchEvent方法,那么result必须为false，再看上面的if条件,
>li != null
 li.mOnTouchListener != null
 (mViewFlags & ENABLED_MASK) == ENABLED
 li.mOnTouchListener.onTouch(this, event)

第一二个条条件 只要注册touch事件就不为null
第三个条件，判断当前点击的控件是否enable
主要的第四个条件，然后我们重写了onTouch方法,就看返回值了

所以只要 onTouch返回false, 那么onTouchEvent就能够执行了，然后看下onTouchEvent(event)方法

   /**
     * Implement this method to handle touch screen motion events.
     * <p>
     * If this method is used to detect click actions, it is recommended that
     * the actions be performed by implementing and calling
     * {@link #performClick()}. This will ensure consistent system behavior,
     * including:
     * <ul>
     * <li>obeying click sound preferences
     * <li>dispatching OnClickListener calls
     * <li>handling {@link AccessibilityNodeInfo#ACTION_CLICK ACTION_CLICK} when
     * accessibility features are enabled
     * </ul>
     *
     * @param event The motion event.
     * @return True if the event was handled, false otherwise.
     */
    public boolean onTouchEvent(MotionEvent event) {
    
      if (((viewFlags & CLICKABLE) == CLICKABLE ||
                (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE) ||
                (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE) {
            switch (action) {
                case MotionEvent.ACTION_UP:
                    boolean prepressed = (mPrivateFlags & PFLAG_PREPRESSED) != 0;
                    if ((mPrivateFlags & PFLAG_PRESSED) != 0 || prepressed) {
                        // take focus if we don't have it already and we should in
                        // touch mode.
                        boolean focusTaken = false;
                        if (isFocusable() && isFocusableInTouchMode() && !isFocused()) {
                            focusTaken = requestFocus();
                        }

                        if (prepressed) {
                            // The button is being released before we actually
                            // showed it as pressed.  Make it show the pressed
                            // state now (before scheduling the click) to ensure
                            // the user sees it.
                            setPressed(true, x, y);
                       }

                        if (!mHasPerformedLongPress && !mIgnoreNextUpEvent) {
                            // This is a tap, so remove the longpress check
                            removeLongPressCallback();

                            // Only perform take click actions if we were in the pressed state
                            if (!focusTaken) {
                                // Use a Runnable and post this rather than calling
                                // performClick directly. This lets other visual state
                                // of the view update before click actions start.
                                if (mPerformClick == null) {
                                    mPerformClick = new PerformClick();
                                }
                                if (!post(mPerformClick)) {
                                    performClick();
                                }
                            }
                        }

                        if (mUnsetPressedState == null) {
                            mUnsetPressedState = new UnsetPressedState();
                        }

                        if (prepressed) {
                            postDelayed(mUnsetPressedState,
                                    ViewConfiguration.getPressedStateDuration());
                        } else if (!post(mUnsetPressedState)) {
                            // If the post failed, unpress right now
                            mUnsetPressedState.run();
                        }

                        removeTapCallback();
                    }
                    mIgnoreNextUpEvent = false;
                    break;

                case MotionEvent.ACTION_DOWN:
                    mHasPerformedLongPress = false;

                    if (performButtonActionOnTouchDown(event)) {
                        break;
                    }

                    // Walk up the hierarchy to determine if we're inside a scrolling container.
                    boolean isInScrollingContainer = isInScrollingContainer();

                    // For views inside a scrolling container, delay the pressed feedback for
                    // a short period in case this is a scroll.
                    if (isInScrollingContainer) {
                        mPrivateFlags |= PFLAG_PREPRESSED;
                        if (mPendingCheckForTap == null) {
                            mPendingCheckForTap = new CheckForTap();
                        }
                        mPendingCheckForTap.x = event.getX();
                        mPendingCheckForTap.y = event.getY();
                        postDelayed(mPendingCheckForTap, ViewConfiguration.getTapTimeout());
                    } else {
                        // Not inside a scrolling container, so show the feedback right away
                        setPressed(true, x, y);
                        checkForLongClick(0, x, y);
                    }
                    break;

                case MotionEvent.ACTION_CANCEL:
                    setPressed(false);
                    removeTapCallback();
                    removeLongPressCallback();
                    mInContextButtonPress = false;
                    mHasPerformedLongPress = false;
                    mIgnoreNextUpEvent = false;
                    break;

                case MotionEvent.ACTION_MOVE:
                    drawableHotspotChanged(x, y);

                    // Be lenient about moving outside of buttons
                    if (!pointInView(x, y, mTouchSlop)) {
                        // Outside button
                        removeTapCallback();
                        if ((mPrivateFlags & PFLAG_PRESSED) != 0) {
                            // Remove any future long press/tap checks
                            removeLongPressCallback();

                            setPressed(false);
                        }
                    }
                    break;
            }

            return true;
        }
    
    }

在代码可以看到　MotionEvent.ACTION_UP:抬起手经过判断会执行 performClick(),
 然后看下performClick();
 
     /**
     * Call this view's OnClickListener, if it is defined.  Performs all normal
     * actions associated with clicking: reporting accessibility event, playing
     * a sound, etc.
     *
     * @return True there was an assigned OnClickListener that was called, false
     *         otherwise is returned.
     */
    public boolean performClick() {
        final boolean result;
        final ListenerInfo li = mListenerInfo;
        if (li != null && li.mOnClickListener != null) {
            playSoundEffect(SoundEffectConstants.CLICK);
            li.mOnClickListener.onClick(this);
            result = true;
        } else {
            result = false;
        }

        sendAccessibilityEvent(AccessibilityEvent.TYPE_VIEW_CLICKED);
        return result;
    }

可以看到执行了　onClick()方法

综上所述
* 如果onTouch返回false,View的dispatchTouchEvent方法里面的onTouchEvent(event))就会得到执行,继而执行onclick事件，
* 反之onTouch返回true(该事件被onTouch消费掉) ,就不会走onTouchEvent方法

http://blog.csdn.net/carson_ho/article/details/54136311#t28
