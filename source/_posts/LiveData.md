---
title: LiveData
comments: true
date: 2020-06-15 11:20:42
tags:
categories: Jetpack
---



#### 优势

1. 确保界面符合数据状态
2. 不会发生内存泄漏
3. 不会因Activity停止而导致崩溃
4. 不再需要手动处理生命周期
5. 数据始终保持最新状态
6. 适当的配置更改
7. 共享资源



#### livedata和viewmodel关系

viewmodel中的数据发生变化时通知页面



#### LiveData原理

注册观察者

```java
   public void observe(@NonNull LifecycleOwner owner, @NonNull Observer<? super T> observer) {
        assertMainThread("observe");
        if (owner.getLifecycle().getCurrentState() == DESTROYED) {
            // ignore
            return;
        }
        LifecycleBoundObserver wrapper = new LifecycleBoundObserver(owner, observer);
        ObserverWrapper existing = mObservers.putIfAbsent(observer, wrapper);
        if (existing != null && !existing.isAttachedTo(owner)) {
            throw new IllegalArgumentException("Cannot add the same observer"
                    + " with different lifecycles");
        }
        if (existing != null) {
            return;
        } 
        owner.getLifecycle().addObserver(wrapper); //在这里注册
    }
```





给观察者发送消息

```java
void dispatchingValue(@Nullable ObserverWrapper initiator) {
    if (mDispatchingValue) {
        mDispatchInvalidated = true;
        return;
    }
    mDispatchingValue = true;
    do {
        mDispatchInvalidated = false;
        if (initiator != null) {
            considerNotify(initiator);
            initiator = null;
        } else {
            for (Iterator<Map.Entry<Observer<? super T>, ObserverWrapper>> iterator =
                    mObservers.iteratorWithAdditions(); iterator.hasNext(); ) { //遍历观察者，给观察者发送消息
                considerNotify(iterator.next().getValue());
                if (mDispatchInvalidated) {
                    break;
                }
            }
        }
    } while (mDispatchInvalidated);
    mDispatchingValue = false;
}
```



#### 数据粘性

经过了解和使用 view model,databiding后，我觉得这个不是官方留的坑，在特定的场景可能还需要这样做，我获取手环睡眠数据此时有其他fragment还没创建，我就需要创建后才就监听到数据。





##### 方法1

我们能不能再创建新的ObserverWrapper的时候，直接把mVersion的值赋给mLastVersion，这样就符合(observer.mLastVersion >= mVersion)这一条件了，就不会再继续执行onChanged方法了

https://blog.csdn.net/geyuecang/article/details/89028283



##### 方法2

 singleLiveEvent

其实这个方法解决的并不是粘性事件的问题，而是“数据倒灌”的问题。“数据倒灌”一词出自[KunMinX](https://links.jianshu.com/go?to=https%3A%2F%2Fxiaozhuanlan.com%2Fu%2Fkunminx)的Blog[**重学安卓：LiveData 数据倒灌 背景缘由全貌 独家解析**](https://links.jianshu.com/go?to=https%3A%2F%2Fxiaozhuanlan.com%2Ftopic%2F6719328450),即在setValue后,observe对此次set的value值会进行多次消费。比如进行第二次observe的时候获取到的数据是第一次的旧数据。这样会带来不可预期的后果。



#### 方法三

UnPeekLiveData

```java
public class ProtectedUnPeekLiveData<T> extends LiveData<T> {

    protected boolean isAllowNullValue;

    private final HashMap<Integer, Boolean> observers = new HashMap<>();

    public void observeInActivity(@NonNull AppCompatActivity activity, @NonNull Observer<? super T> observer) {
        LifecycleOwner owner = activity;
        Integer storeId = System.identityHashCode(observer);//源码这里是activity.getViewModelStore()，是为了保证同一个ViewModel环境下"唯一可信源"
        observe(storeId, owner, observer);
    }

    private void observe(@NonNull Integer storeId,
                         @NonNull LifecycleOwner owner,
                         @NonNull Observer<? super T> observer) {

        if (observers.get(storeId) == null) {
            observers.put(storeId, true);
        }

        super.observe(owner, t -> {
            if (!observers.get(storeId)) {
                observers.put(storeId, true);
                if (t != null || isAllowNullValue) {
                    observer.onChanged(t);
                }
            }
        });
    }
    
    @Override
    protected void setValue(T value) {
        if (value != null || isAllowNullValue) {
            for (Map.Entry<Integer, Boolean> entry : observers.entrySet()) {
                entry.setValue(false);
            }
            super.setValue(value);
        }
    }

    protected void clear() {
        super.setValue(null);
    }
}
```



，为每个传入的observer对象携带一个布尔类型的值，作为其是否能进入observe方法的开关。每当有一个新的observer存进来的时候，开关默认关闭。

每次setValue后，打开所有Observer的开关，允许所有observe执行。

这个确实清晰，简单



https://www.jianshu.com/p/d0244c4c7cc9

https://juejin.cn/post/6844903623252508685



UnPeekLiveData

或者 ELiveData 

https://www.ershicimi.com/p/2f57df7148300017e8a0f7227720a2e8

https://juejin.im/post/5b2b1b2cf265da5952314b63

https://www.jianshu.com/p/79d909b6f8bd

https://juejin.im/post/6892704779781275662



https://www.jianshu.com/p/d0244c4c7cc9