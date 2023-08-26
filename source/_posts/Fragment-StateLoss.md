---
title: Fragment_StateLoss
date: 2022-09-15 10:25:12
tags: Fragment
categories:  ANDROID
---



问题 ，Activity恢复后 mStateSaved,是怎么恢复的？





就是onSavedInstanceState 的官方API文档2

这里问题的场景是Activity已经关闭了,rpc请求很久再回来,导致了这个问题,所以我觉得判断livedate激活状态就可以了

可是这个改动应该只能在resume吧？

LiveData started 和 resumed 都可以

Livedata 会感知activity生命周期

这个有可能 activity 也在 started / resumed 状态下发出吧？ 这样的话，showDialog 应该也还会崩溃，因为onSavedInstanceState 是在 paused 时候发生的，这个时候应该还是 started 状态。

```
/**

* 后续考虑做个技改把它加到[BaseDialogFragment]里面

*/

fun showWithLifecycle(fragmentActivity: FragmentActivity, tag: String? = null) {

  val liveData = showWithLifecycleLiveData ?: MutableLiveData<Boolean>().also {

    it.observe(fragmentActivity) { this.show(fragmentActivity.supportFragmentManager, tag) }

    showWithLifecycleLiveData = it

  }

  liveData.value = true

}
```







onSaveInstanceState保存分析

https://juejin.cn/post/6995791487426363405



状态恢复

https://www.jianshu.com/p/58579627f70a



https://juejin.cn/post/7057564670567120903



 **can not perform this action after onsaveinstancestate dialogfragment show** 





developer  -> 不保留活动 



onSaveInstanceState以下5种情况被调用：

1、当用户按下手机home键的时候。

2、长按手机home键或者按下菜单键时。

3、手机息屏时。

4、FirstActivity启动SecondActivity，FirstActivity就会调用，也就是说打开新Activity时，原Activity就会调用。

5、默认情况下横竖屏切换时。









打开Activity -> Home  -> 再从后台拿出来

![20220919212018](Fragment-StateLoss/20220919212018.jpg)



既然状态的保存与恢复都必须要把Fragment带上，那么一旦当Fragment的状态已保存过了，那么就不应该再改变Fragment的状态。因此FragmentManager的每一个操作前，都会调用一个方法来检查状态是否保存过了：

```java
private void checkStateLoss() {
    if (mStateSaved) {
        throw new IllegalStateException(
                    "Can not perform this action after onSaveInstanceState");
    }
    if (mNoTransactionsBecause != null) {
        throw new IllegalStateException(
                    "Can not perform this action inside of " + mNoTransactionsBecause);
    }
}
```





```java
/**
 * This is the same as {@link #onSaveInstanceState} but is called for activities
 * created with the attribute {@link android.R.attr#persistableMode} set to
 * <code>persistAcrossReboots</code>. The {@link android.os.PersistableBundle} passed
 * in will be saved and presented in {@link #onCreate(Bundle, PersistableBundle)}
 * the first time that this activity is restarted following the next device reboot.
 *
 * @param outState Bundle in which to place your saved state.
 * @param outPersistentState State which will be saved across reboots.
 *
 * @see #onSaveInstanceState(Bundle)
 * @see #onCreate
 * @see #onRestoreInstanceState(Bundle, PersistableBundle)
 * @see #onPause
 */
public void onSaveInstanceState(@NonNull Bundle outState,
        @NonNull PersistableBundle outPersistentState) {
    onSaveInstanceState(outState);
}
```



为什么会有这个异常



https://blog.csdn.net/lzf_acraftsman/article/details/108831949

https://juejin.cn/post/6844903458605105160



fragment重叠原因





 

####  Fragment懒加载

Fragment和ViewPager一起使用会有个预加载机制，会把旁白的Fragment的生命周期方法
前半段先执行，然后执行自身的生命周期方法

在项目终从其他页面回到MainAcitivty的时候，三个页面的生命周期方法都跑了一遍

```
　  D/FinanceFragment         Test: onStart()
    D/WealthFragment         Test: onStart()
    D/MineFragment         Test: onStart()
    D/FinanceFragment         Test: onResume()
    D/WealthFragment         Test: onResume()
    D/MineFragment         Test: onResume()
```


```
     
    private boolean isPrepared;  //判断view是否加载完成,在视图未初始化的时候，在lazyLoad当中就使用的话，就会有空指针的异常
    private boolean isVisible;  //判断当前Fragment是否可见状态,标志已经初始化完成，因为setUserVisibleHint是在onCreateView之前调用的
	  @Override
    public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        isPrepared = true;
        lazyLoad();
    }


    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
//        Log.d(TAG + "         Test", " onCreateView()");

        if (rootView == null) {
            int view = setLayoutId();
            if (view != 0) {
                rootView = inflater.inflate(view, container, false);
            }
        } else {
            ViewGroup parent = (ViewGroup) rootView.getParent();
            if (parent != null) {
                parent.removeView(rootView);
            }
        }
        initView(rootView);
        return rootView;
    }

    //  http://www.10tiao.com/html/565/201702/2247483988/1.html
    // 标志位，标志已经初始化完成，因为setUserVisibleHint是在onCreateView之前调用的，
    // 在视图未初始化的时候，在lazyLoad当中就使用的话，就会有空指针的异常
    private boolean isPrepared;
    //标志当前页面是否可见
    private boolean isVisible;

    @Override
    public void setUserVisibleHint(boolean isVisibleToUser) {
        super.setUserVisibleHint(isVisibleToUser);
//        Log.d(TAG + "         Test", " setUserVisibleHint() is Visible : ?  " + isVisibleToUser);

        if (getUserVisibleHint()) {
            isVisible = true;
            onVisible();
        } else {
            isVisible = false;
            onInvisible();
        }
    }

    protected void onInvisible() {

    }

    protected void onVisible() {
        lazyLoad();
    }

    private void lazyLoad() {
        if (!isVisible || !isPrepared) {
            return;
        }
        requestData();
    }

    /**
     * 请求数据
     */
    protected void requestData() {
        Log.d(TAG + "         Test", " requestData ");
    }


```

 http://www.10tiao.com/html/565/201702/2247483988/1.html

####  Activity  dialogFragment 的事件回调


```
    interface ISelectListener {
        fun getItemPosition(position: Int)
    }

    var mListener: ISelectListener? = null

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (parentFragment is ISelectListener){
            mListener = parentFragment  as ISelectListener
        }else if (context is ISelectListener) {
            mListener = context
        } else {
            throw RuntimeException(context!!.toString() + " must implement ISelectListener")
        }
    }
```

https://developer.android.com/guide/components/fragments.html



onHiddenChanged切换刷新

使用hide()/show()发现生命周期基本不执行，不过可以用到这个onHiddenChanged();

看下执行的生命周期;   从 SecondFragment 页面开始到 ->FirstFragment 

> 07-18 15:53:25.128 7758-7758/com.mineutils D/SecondFragment: onAttach(Context context)
> 07-18 15:53:25.129 7758-7758/com.mineutils D/SecondFragment: onCreate()
> 07-18 15:53:25.142 7758-7758/com.mineutils D/SecondFragment: onCreateView()
> 07-18 15:53:25.148 7758-7758/com.mineutils D/SecondFragment:  onViewCreated
> 															  onActivityCreated()
> 															  onStart()
> ​    															  onResume()
>
> 07-18 15:53:34.200 7758-7758/com.mineutils D/SecondFragment: onHiddenChanged hidden   true
> 07-18 15:53:34.207 7758-7758/com.mineutils D/FirstFragment:    onCreateView()
> 07-18 15:53:34.208 7758-7758/com.mineutils D/FirstFragment:    onViewCreated
> 												 		   onActivityCreated()
> ​														   onStart()
>
> 07-18 15:53:53.968 7758-7758/com.mineutils D/FirstFragment: onHiddenChanged hidden   true
> 07-18 15:53:53.968 7758-7758/com.mineutils D/SecondFragment: onHiddenChanged hidden   false



https://blog.csdn.net/cml_blog/article/details/41411451



#### 导航栏

https://wizardforcel.gitbooks.io/w3school-android/content/77.html

https://juejin.im/post/5901b564570c35005804424b

