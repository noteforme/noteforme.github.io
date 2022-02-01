---
title: TouchEvent2
comments: true
date: 2017-08-13 22:01:04
tags: TouchEvent
categories: VIEW

---
 **Android touch Event**

 事件分发其实就是对MotionEvent事件得分发过程,当MotionEvet产生后，系统需要把这个事件传递给一个具体得View,而这个传递得过程就是分发过程

概述

**为什么会有事件分发机制?**

Android得View是树形结构，绘制得时候遍历子View,他们必定是会重合分布，那么当我
们点击得时候，是应该谁来响应这个事件呢? 然后才有事件分发

> Activity －> PhoneWindow －> DecorView －> ViewGroup －> ... －> View

整个分发的宏观过程,*Android群英传*　这本书描述的很清晰,然后配合　Android开发艺术探索，的源码分析，简直天作之合．

主要方法

####  method

事件分发机制事由　Activity -> ViewGroup -> View　方向上得传递
通过三个很重要得方法共同完成 dispatchTouchEvent、onInterceptTouchEvent、onTouchEvent

- dispatchTouchEvent()

| 属性 | 介绍 |
|--------|--------|
|   使用对象 | Activity、ViewGroup、View     |
|作用|事件分发|

- onInterceptTouchEvent()

| 属性 | 介绍 |
|--------|--------|
|    作用    |  拦截事件  |
|调用时刻 |在ViewGroup的dispatchTouchEvent()内部调用|

- onTouchEvent()

| 属性 | 介绍 |
|--------|--------|
|    使用对象    |  Activity、ViewGroup、View      |
|作用 |事件消费|
|调用时刻   |在dispatchTouchEvent()内部调用|

三个方法的关系

  可以用 singwhatiwanna 写的伪代码表示:

```
    public boolean dispatchTouchEvent(MotionEvent ev){
   	    boolean consume = false;
   	    if(onInterceptTouchEvent(ev)){
            	consume = onTouchEvent(ev);	    
   	    }else{
   	     	 consume = child.dispatchTouchEvent(ev);
   	    }
   	    return consume;
    }
```
-   dispatchTouchEvent　：对于ViewGroup，点击事件产生后首先会传递给它，
-   onInterceptTouchEvent(ev）: 如果方法返回true,则拦截事件,事件就交给当前ViewGroup处理,接着调用当前
　　　　ViewGroup的onTouchEvent.如果返回flase,就交给它的子元素,子元素的dispatchTouchEvent就会调用
-  onTouchEvent : 如果返回false，那么父容器的onTouchEvent将会被调用.

#### Activity ViewGroup View

##### source code

* Activity

  ```
  public boolean dispatchTouchEvent(MotionEvent ev) {
      if (ev.getAction() == MotionEvent.ACTION_DOWN) {
          onUserInteraction();
      }
      if (getWindow().superDispatchTouchEvent(ev)) {
          return true;
      }
      return onTouchEvent(ev);
  }
  ```

* ViewGroup

```
@Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        if (mInputEventConsistencyVerifier != null) {
            mInputEventConsistencyVerifier.onTouchEvent(ev, 1);
        }

        // If the event targets the accessibility focused view and this is it, start
        // normal event dispatch. Maybe a descendant is what will handle the click.
        if (ev.isTargetAccessibilityFocus() && isAccessibilityFocusedViewOrHost()) {
            ev.setTargetAccessibilityFocus(false);
        }

        boolean handled = false;
        if (onFilterTouchEventForSecurity(ev)) {
            final int action = ev.getAction();
            final int actionMasked = action & MotionEvent.ACTION_MASK;

            // Handle an initial down.
            if (actionMasked == MotionEvent.ACTION_DOWN) {
                // Throw away all previous state when starting a new touch gesture.
                // The framework may have dropped the up or cancel event for the previous gesture
                // due to an app switch, ANR, or some other state change.
                cancelAndClearTouchTargets(ev);
                resetTouchState();
            }

            // Check for interception.
            final boolean intercepted;
            if (actionMasked == MotionEvent.ACTION_DOWN
                    || mFirstTouchTarget != null) {
                final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
                if (!disallowIntercept) {
                    intercepted = onInterceptTouchEvent(ev);
                    ev.setAction(action); // restore action in case it was changed
                } else {
                    intercepted = false;
                }
            } else {
                // There are no touch targets and this action is not an initial down
                // so this view group continues to intercept touches.
                intercepted = true;
            }

            // If intercepted, start normal event dispatch. Also if there is already
            // a view that is handling the gesture, do normal event dispatch.
            if (intercepted || mFirstTouchTarget != null) {
                ev.setTargetAccessibilityFocus(false);
            }

            // Check for cancelation.
            final boolean canceled = resetCancelNextUpFlag(this)
                    || actionMasked == MotionEvent.ACTION_CANCEL;

            // Update list of touch targets for pointer down, if needed.
            final boolean split = (mGroupFlags & FLAG_SPLIT_MOTION_EVENTS) != 0;
            TouchTarget newTouchTarget = null;
            boolean alreadyDispatchedToNewTouchTarget = false;
            if (!canceled && !intercepted) {

                // If the event is targeting accessibility focus we give it to the
                // view that has accessibility focus and if it does not handle it
                // we clear the flag and dispatch the event to all children as usual.
                // We are looking up the accessibility focused host to avoid keeping
                // state since these events are very rare.
                View childWithAccessibilityFocus = ev.isTargetAccessibilityFocus()
                        ? findChildWithAccessibilityFocus() : null;

                if (actionMasked == MotionEvent.ACTION_DOWN
                        || (split && actionMasked == MotionEvent.ACTION_POINTER_DOWN)
                        || actionMasked == MotionEvent.ACTION_HOVER_MOVE) {
                    final int actionIndex = ev.getActionIndex(); // always 0 for down
                    final int idBitsToAssign = split ? 1 << ev.getPointerId(actionIndex)
                            : TouchTarget.ALL_POINTER_IDS;

                    // Clean up earlier touch targets for this pointer id in case they
                    // have become out of sync.
                    removePointersFromTouchTargets(idBitsToAssign);

                    final int childrenCount = mChildrenCount;
                    if (newTouchTarget == null && childrenCount != 0) {
                        final float x = ev.getX(actionIndex);
                        final float y = ev.getY(actionIndex);
                        // Find a child that can receive the event.
                        // Scan children from front to back.
                        final ArrayList<View> preorderedList = buildTouchDispatchChildList();
                        final boolean customOrder = preorderedList == null
                                && isChildrenDrawingOrderEnabled();
                        final View[] children = mChildren;
                        for (int i = childrenCount - 1; i >= 0; i--) {
                            final int childIndex = getAndVerifyPreorderedIndex(
                                    childrenCount, i, customOrder);
                            final View child = getAndVerifyPreorderedView(
                                    preorderedList, children, childIndex);

                            // If there is a view that has accessibility focus we want it
                            // to get the event first and if not handled we will perform a
                            // normal dispatch. We may do a double iteration but this is
                            // safer given the timeframe.
                            if (childWithAccessibilityFocus != null) {
                                if (childWithAccessibilityFocus != child) {
                                    continue;
                                }
                                childWithAccessibilityFocus = null;
                                i = childrenCount - 1;
                            }

                            if (!child.canReceivePointerEvents()
                                    || !isTransformedTouchPointInView(x, y, child, null)) {
                                ev.setTargetAccessibilityFocus(false);
                                continue;
                            }

                            newTouchTarget = getTouchTarget(child);
                            if (newTouchTarget != null) {
                                // Child is already receiving touch within its bounds.
                                // Give it the new pointer in addition to the ones it is handling.
                                newTouchTarget.pointerIdBits |= idBitsToAssign;
                                break;
                            }

                            resetCancelNextUpFlag(child);
                            if (dispatchTransformedTouchEvent(ev, false, child, idBitsToAssign)) {
                                // Child wants to receive touch within its bounds.
                                mLastTouchDownTime = ev.getDownTime();
                                if (preorderedList != null) {
                                    // childIndex points into presorted list, find original index
                                    for (int j = 0; j < childrenCount; j++) {
                                        if (children[childIndex] == mChildren[j]) {
                                            mLastTouchDownIndex = j;
                                            break;
                                        }
                                    }
                                } else {
                                    mLastTouchDownIndex = childIndex;
                                }
                                mLastTouchDownX = ev.getX();
                                mLastTouchDownY = ev.getY();
                                newTouchTarget = addTouchTarget(child, idBitsToAssign);
                                alreadyDispatchedToNewTouchTarget = true;
                                break;
                            }

                            // The accessibility focus didn't handle the event, so clear
                            // the flag and do a normal dispatch to all children.
                            ev.setTargetAccessibilityFocus(false);
                        }
                        if (preorderedList != null) preorderedList.clear();
                    }

                    if (newTouchTarget == null && mFirstTouchTarget != null) {
                        // Did not find a child to receive the event.
                        // Assign the pointer to the least recently added target.
                        newTouchTarget = mFirstTouchTarget;
                        while (newTouchTarget.next != null) {
                            newTouchTarget = newTouchTarget.next;
                        }
                        newTouchTarget.pointerIdBits |= idBitsToAssign;
                    }
                }
            }

            // Dispatch to touch targets.
            if (mFirstTouchTarget == null) {
                // No touch targets so treat this as an ordinary view.
                handled = dispatchTransformedTouchEvent(ev, canceled, null,
                        TouchTarget.ALL_POINTER_IDS);
            } else {
                // Dispatch to touch targets, excluding the new touch target if we already
                // dispatched to it.  Cancel touch targets if necessary.
                TouchTarget predecessor = null;
                TouchTarget target = mFirstTouchTarget;
                while (target != null) {
                    final TouchTarget next = target.next;
                    if (alreadyDispatchedToNewTouchTarget && target == newTouchTarget) {
                        handled = true;
                    } else {
                        final boolean cancelChild = resetCancelNextUpFlag(target.child)
                                || intercepted;
                        if (dispatchTransformedTouchEvent(ev, cancelChild,
                                target.child, target.pointerIdBits)) {
                            handled = true;
                        }
                        if (cancelChild) {
                            if (predecessor == null) {
                                mFirstTouchTarget = next;
                            } else {
                                predecessor.next = next;
                            }
                            target.recycle();
                            target = next;
                            continue;
                        }
                    }
                    predecessor = target;
                    target = next;
                }
            }

            // Update list of touch targets for pointer up or cancel, if needed.
            if (canceled
                    || actionMasked == MotionEvent.ACTION_UP
                    || actionMasked == MotionEvent.ACTION_HOVER_MOVE) {
                resetTouchState();
            } else if (split && actionMasked == MotionEvent.ACTION_POINTER_UP) {
                final int actionIndex = ev.getActionIndex();
                final int idBitsToRemove = 1 << ev.getPointerId(actionIndex);
                removePointersFromTouchTargets(idBitsToRemove);
            }
        }

        if (!handled && mInputEventConsistencyVerifier != null) {
            mInputEventConsistencyVerifier.onUnhandledEvent(ev, 1);
        }
        return handled;
    }
```

* View

  ```
  public boolean dispatchTouchEvent(MotionEvent event) {
      // If the event should be handled by accessibility focus first.
      if (event.isTargetAccessibilityFocus()) {
          // We don't have focus or no virtual descendant has it, do not handle the event.
          if (!isAccessibilityFocusedViewOrHost()) {
              return false;
          }
          // We have focus and got the event, then use normal event dispatch.
          event.setTargetAccessibilityFocus(false);
      }
  
      boolean result = false;
  
      if (mInputEventConsistencyVerifier != null) {
          mInputEventConsistencyVerifier.onTouchEvent(event, 0);
      }
  
      final int actionMasked = event.getActionMasked();
      if (actionMasked == MotionEvent.ACTION_DOWN) {
          // Defensive cleanup for new gesture
          stopNestedScroll();
      }
  
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
  
      if (!result && mInputEventConsistencyVerifier != null) {
          mInputEventConsistencyVerifier.onUnhandledEvent(event, 0);
      }
  
      // Clean up after nested scrolls if this is the end of a gesture;
      // also cancel it if we tried an ACTION_DOWN but we didn't want the rest
      // of the gesture.
      if (actionMasked == MotionEvent.ACTION_UP ||
              actionMasked == MotionEvent.ACTION_CANCEL ||
              (actionMasked == MotionEvent.ACTION_DOWN && !result)) {
          stopNestedScroll();
      }
  
      return result;
  }
  ```







##### 图ACTION_DOWN分析

1.MotionEvent.ACTION_DOWN 都不拦截	

 Activity		  	dispatchTouchEvent()	1 => 5

 RootView 	  dispatchTouchEvent()	29 =>	 (mGroupFlags & FLAG_DISALLOW_INTERCEPT)==0

​							=> 33 => 58 => 68 => 122 =>	dispatchTransformedTouchEvent有子view重复上面流程  => dispatchTransformedTouchEvent没有子View =>

View1					dispatchTouchEvent() 36 => 54 result=false =>事件开始往回传=>

ViewGroupA		122=>163=> 164 dispatchTransformedTouchEvent child传null =>开始调用View dispatchTouchEvent()=false =>  事件继续回传=> 

RootView			=>重复上面ViewGroupA的流程 => 164 handled=false =>

Activity					=>5  getWindow().superDispatchTouchEvent(ev)   => onTouchEvent(ev);



##### 图ACTION_MOVE UP分析 RootView  onInterceptTouchEvent =true



 Activity		  	dispatchTouchEvent()	1 => 5

 RootView 	  dispatchTouchEvent()	29 =>	 (mGroupFlags & FLAG_DISALLOW_INTERCEPT)==0 => 164 >handler = false =>

 Activity					=>5  getWindow().superDispatchTouchEvent(ev)   => onTouchEvent(ev);



##### 图ACTION_MOVE UP分析 View touchevent = true

 Activity		  	dispatchTouchEvent()	1 => 5

 RootView 	  dispatchTouchEvent()	29 =>	41 => 170 => 183 遍历查找消费event的View => handled = true

 Activity					=>5  getWindow().superDispatchTouchEvent(ev)   => onTouchEvent(ev);

 

##  View点击事件处理(不是ViewGroup)

-   onTouch返回false

```
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
```
打印结果:
 >D/DispatchActivity: 执行 button2 onTouch() 动作0
 D/DispatchActivity: 执行 button2 onTouch() 动作1
 D/DispatchActivity: you clicked button2

-  onTouch返回true
Log打印结果:
> D/DispatchActivity: 执行 button2 onTouch() 动作0
 D/DispatchActivity: 执行 button2 onTouch() 动作1

不同得返回值,得到不同得结果　看下为什么
看下　View的dispatchTouchEvent（）　API  23 源码的事件分发流程
点击 Button寻找dispatchTouchEvent() `Button -> dispatchTouchEvent() ->  TextView->View `
 (点击Button后就会找 dispatchTouchEvent()，Button没有，然后就去父类TextView还是没找到，最后在View找到了)

``` 
	 /**
     * Pass the touch screen motion event down to the target view, or this
     * view if it is the target.
     *
     * @param event The motion event to be dispatched.
     * @return True if the event was handled by the view, false otherwise.
     */
    public boolean dispatchTouchEvent(MotionEvent event) {
        // If the event should be handled by accessibility focus first.
        if (event.isTargetAccessibilityFocus()) {
            // We don't have focus or no virtual descendant has it, do not handle the event.
            if (!isAccessibilityFocusedViewOrHost()) {
                return false;
            }
            // We have focus and got the event, then use normal event dispatch.
            event.setTargetAccessibilityFocus(false);
        }

        boolean result = false;

        if (mInputEventConsistencyVerifier != null) {
            mInputEventConsistencyVerifier.onTouchEvent(event, 0);
        }

        final int actionMasked = event.getActionMasked();
        if (actionMasked == MotionEvent.ACTION_DOWN) {
            // Defensive cleanup for new gesture
            stopNestedScroll();
        }

        if (onFilterTouchEventForSecurity(event)) {
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

        if (!result && mInputEventConsistencyVerifier != null) {
            mInputEventConsistencyVerifier.onUnhandledEvent(event, 0);
        }

        // Clean up after nested scrolls if this is the end of a gesture;
        // also cancel it if we tried an ACTION_DOWN but we didn't want the rest
        // of the gesture.
        if (actionMasked == MotionEvent.ACTION_UP ||
                actionMasked == MotionEvent.ACTION_CANCEL ||
                (actionMasked == MotionEvent.ACTION_DOWN && !result)) {
            stopNestedScroll();
        }

        return result;
    }
```

- 分析 
  第34行,result默认为false,li.mOnTouchListener.onTouch(this, event))默认也是false,接着
  第40行,onTouchEvent(event)) 方法也会得到执行 ->  performClick()会给点击事件  一个回调;
               
           

**以下内容参考**: [gcssloop](http://www.gcssloop.com/customview/dispatch-touchevent-theory) 
ViewGroup的　[dispatchTouchEvent()解析](http://wangkuiwu.github.io/2015/01/04/TouchEvent-ViewGroup/) 
http://blog.csdn.net/guolin_blog/article/details/9097463



https://mp.weixin.qq.com/s/KU32XpwDFBOl8ueXIaA8Tw