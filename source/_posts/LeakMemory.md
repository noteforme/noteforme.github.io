---
title: LeakMemory
comments: true
date: 2018-02-09 16:15:17
tags:
categories: ANDROID
---



Android内存泄漏的可能方式

https://www.jianshu.com/p/ac00e370f83d

[内存泄漏总结](https://github.com/francistao/LearningNotes/blob/master/Part1/Android/Android%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93.md)

```
public class SampleActivity extends Activity {

  /**
   * Instances of static inner classes do not hold an implicit
   * reference to their outer class.
   */
  private static class MyHandler extends Handler {
    private final WeakReference<SampleActivity> mActivity;

    public MyHandler(SampleActivity activity) {
      mActivity = new WeakReference<SampleActivity>(activity);
    }

    @Override
    public void handleMessage(Message msg) {
      SampleActivity activity = mActivity.get();
      if (activity != null) {
        // ...
      }
    }
  }

  private final MyHandler mHandler = new MyHandler(this);

  /**
   * Instances of anonymous classes do not hold an implicit
   * reference to their outer class when they are "static".
   */
  private static final Runnable sRunnable = new Runnable() {
      @Override
      public void run() { /* ... */ }
  };

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Post a message and delay its execution for 10 minutes.
    mHandler.postDelayed(sRunnable, 1000 * 60 * 10);
    
    // Go back to the previous Activity.
    finish();
  }
}
```

* kotin Fragment方式

  ```
    class MyHandler : Handler {
          private var mFragment: WeakReference<Any>
  
          constructor(fragment: Fragment) {
              mFragment = WeakReference(fragment)
          }
  
          override fun handleMessage(msg: android.os.Message?) {
              super.handleMessage(msg)
              val fragment = mFragment.get() as PayProFragment
              fragment.reloadPay()
          }
      }
  ```

  

* [内存泄漏处理-Handler](https://www.jianshu.com/p/63aead89f3b9)

* [查找方式](https://juejin.im/entry/589542ed2f301e0069054007)

  https://www.jianshu.com/p/bdfd2a6b2681

​    https://developer.android.com/studio/profile/memory-profiler