---
title: Fragment
comments: true
date: 2017-10-05 19:54:50
tags:
categories:  ANDROID
---

https://developer.android.com/reference/android/app/Fragment.html

https://developer.android.com/guide/components/fragments.html?hl=zh-cn

https://wizardforcel.gitbooks.io/w3school-android/content/77.html
https://juejin.im/post/5901b564570c35005804424b

####  Fragment包的区别

fragment有两种继承方式
android.support.v4.app.Fragment和android.app.Fragment

下面作者　总结的不错　干脆直接用了

> android.app.Fragment 兼容的最低版本是android:minSdkVersion="11" 即3.0版
> android.support.v4.app.Fragment 兼容的最低版本是android:minSdkVersion="4" 即1.6版

> android.app.Fragment使用 (ListFragment)getFragmentManager().findFragmentById(R.id.userList) 获得 ，继承Activity
> android.support.v4.app.Fragment使用 (ListFragment)getSupportFragmentManager().findFragmentById(R.id.userList) 获得 ，需要继承android.support.v4.app.FragmentActivity


Android Support兼容包

Support Library
我们都知道Android一些SDK比较分裂，为此google官方提供了Android Support Library package 系列的包来保证高版本sdk开发的向下兼容性, 所以你可能经常看到v4，v7，v13这些数字，首先我们就来理清楚这些数字的含义，以及它们之间的区别。

1. support-v4

   用在API lever 4(即Android 1.6)或者更高版本之上。它包含了相对更多的内容，而且用的更为广泛，例如：Fragment，NotificationCompat，LoadBroadcastManager，ViewPager，PageTabAtrip，Loader，FileProvider 等
   Gradle引用方法：

compile 'com.android.support:support-v4:21.0.3'

2. support-v7

   这个包是为了考虑API level 7(即Android 2.1)及以上版本而设计的，但是v7是要依赖v4这个包的，v7支持了Action Bar以及一些Theme的兼容。
   Gradle引用方法:compile 'com.android.support:appcompat-v7:21.0.3'



3. support-v13

   这个包的设计是为了API level 13(即Android 3.2)及更高版本的，一般我们都不常用，平板开发中能用到，这里就不过多介绍了。

   作者：天天想念
   链接：http://www.jianshu.com/p/db28adde1c39

#### 添加Fragment



* xml设置

```
   <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:layout_width="match_parent" android:layout_height="match_parent">
     <fragment class="com.example.android.apis.app.FragmentLayout$TitlesFragment"
            android:id="@+id/titles"
            android:layout_width="match_parent" android:layout_height="match_parent" />
</FrameLayout>
```




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
参考: http://www.10tiao.com/html/565/201702/2247483988/1.html

####  Activity 的事件回调


```
    interface OnSetdataListener {
        void setDataMain(Project mProject);
    }

    OnSetdataListener mListener;

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        try {
            mListener = (OnSetdataListener) context;
        } catch (ClassCastException e) {
            throw new ClassCastException(context.toString() + "must implement OnSetdataListener");
        }
    }	

```

https://developer.android.com/guide/components/fragments.html?hl=zh-cn

####  onHiddenChanged

使用hide()/show()发现生命周期基本不执行，不过可以用到这个onHiddenChanged();

看下执行的生命周期;   从 SecondFragment 页面开始到 ->FirstFragment 

> 07-18 15:53:25.128 7758-7758/com.mineutils D/SecondFragment: onAttach(Context context)
> 07-18 15:53:25.129 7758-7758/com.mineutils D/SecondFragment: onCreate()
> 07-18 15:53:25.142 7758-7758/com.mineutils D/SecondFragment: onCreateView()
> 07-18 15:53:25.148 7758-7758/com.mineutils D/SecondFragment:  onViewCreated
>   															  onActivityCreated()
>   															  onStart()
> ​    															  onResume()
>
> 07-18 15:53:34.200 7758-7758/com.mineutils D/SecondFragment: onHiddenChanged hidden   true
> 07-18 15:53:34.207 7758-7758/com.mineutils D/FirstFragment:    onCreateView()
> 07-18 15:53:34.208 7758-7758/com.mineutils D/FirstFragment:    onViewCreated
>   												 		   onActivityCreated()
> ​														   onStart()
>
> 07-18 15:53:53.968 7758-7758/com.mineutils D/FirstFragment: onHiddenChanged hidden   true
> 07-18 15:53:53.968 7758-7758/com.mineutils D/SecondFragment: onHiddenChanged hidden   false



https://blog.csdn.net/cml_blog/article/details/41411451



#### fragment常用特性

##### commitAllowingStateLoss VS  commit()区别

https://huxian99.github.io/2016/08/28/cj3qymo360000owxk9zp17alo/

##### fragment回退栈

tx.addToBackStack(null); 添加回退功能，类似Activity压栈的过程



* Fragment控制父Fragment展示

  https://blog.csdn.net/u011481547/article/details/71552720

