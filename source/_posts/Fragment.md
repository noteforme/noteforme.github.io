---
title: Fragment
comments: true
date: 2017-10-05 19:54:50
tags:
categories:  ANDROID
---



https://developer.android.com/reference/android/app/Fragment.html

https://developer.android.com/guide/components/fragments.html

https://wizardforcel.gitbooks.io/w3school-android/content/77.html
https://juejin.im/post/5901b564570c35005804424b



#### Lifecycle

https://developer.android.com/guide/fragments/lifecycle

* onViewCreated(View view, Bundle savedInstanceState)

Called immediately after `onCreateView(android.view.LayoutInflater, android.view.ViewGroup, android.os.Bundle)` has returned, but before any saved state has been restored in to the view.



<img src="Fragment/fragment-view-lifecycle.png" alt="ff" style="zoom:67%;" />



##### fragment回调

```
var mListener: IFeedBackListener? = null

override fun onAttach(context: Context) {
    super.onAttach(context)
    if (parentFragment is IFeedBackListener) {
        mListener = parentFragment as IFeedBackListener
    } else if (context is IFeedBackListener) {
        mListener = context
    } else {
        throw RuntimeException(context?.toString() + " must implement ISelectListener")
    }
}
```

https://noteforme.github.io/2017/10/05/Fragment/



#### Fragment重叠



 横竖屏切换

被系统回收（复现 进入开发者选项 -》不保留活动）

https://www.jianshu.com/p/c12a98a36b2b

#####  BottomNavigationView error

使用BottomNavigationView出现很严重的问题,很久才找到原因

```
 Caused by: android.view.InflateException: Binary XML file line #16: Binary XML file line #16: Error inflating class com.google.android.material.bottomnavigation.BottomNavigationView
     Caused by: android.view.InflateException: Binary XML file line #16: Error inflating class com.google.android.material.bottomnavigation.BottomNavigationView
```

因为 AndroidManifest.xml  application下的android:theme="@style/AppTheme"多出了,把下面的删除了就好了.

```
<item name="android:textColorSecondary">#ffffff</item>
<item name="android:actionOverflowButtonStyle">@style/OverflowMenuStyle</item>
<item name="android:listDivider">@drawable/divider</item>
```

#####   重叠原因 

>  旋转屏幕时候，创建的homeFragment会被回收,在销毁前，执行了onSaveInstanceState(Bundle outState)这个方法。这个方法会保存activity的一些信息，其中就包括添加过的fragment，Activity重建后，再次初始化homeFragment，接着显示，导致了重叠的问题。



##### 代码验证

```
private fun initFragment() {
    if (homeFragment == null) {
        homeFragment = HomeFragment()
        addFragment(homeFragment!!)
        showFragment(homeFragment)
    }
}

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    Timber.d("onCreate() $homeFragment")
    initFragment()
    Timber.d("initFragment() $homeFragment")   
}

 override fun onDestroy() {
        super.onDestroy()
        Timber.tag(TAG).d("onDestroy() homeFragment $homeFragment")
 }

```

日志

```
 D/TabBottomActivity LaunchModeActivity: onCreate() taskId  641
 D/TabBottomActivity LaunchModeActivity: onCreate() homeFragment null
 D/TabBottomActivity LaunchModeActivity: initFragment() homeFragment HomeFragment{1adc3fa} (b0660244-39c4-4104-b558-f5384c7adb0d) id=0x7f09010e}
 D/TabBottomActivity LaunchModeActivity: onStart()
 D/TabBottomActivity LaunchModeActivity: onResume()
 D/TabBottomActivity LaunchModeActivity: onPause()
 D/TabBottomActivity LaunchModeActivity: onSaveInstanceState()
 D/TabBottomActivity LaunchModeActivity: onStop()
 D/TabBottomActivity LaunchModeActivity: onDestroy()
 D/TabBottomActivity LaunchModeActivity: onDestroy() homeFragment HomeFragment{1adc3fa} (d84f5956-fa2c-4cd2-8397-0904040d2a6f)}
 D/TabBottomActivity LaunchModeActivity: onCreate() taskId  641
 D/TabBottomActivity LaunchModeActivity: onCreate() homeFragment null
 D/TabBottomActivity LaunchModeActivity: initFragment() homeFragment HomeFragment{ac076db} (f8ee348b-6ff7-4b1e-ba27-6e47e848f623) id=0x7f09010e}
 D/TabBottomActivity LaunchModeActivity: onStart()
 D/TabBottomActivity LaunchModeActivity: onRestoreInstanceState()
 D/TabBottomActivity LaunchModeActivity: onResume()
```

从 initFragment（） 1adc3fa , ac076db 可以看到，HomeFrgment重新创建了.

##### 处理方法

1. onSaveInstanceState 不保存信息

   ```kotlin
      override fun onSaveInstanceState(outState: Bundle) {
   //        super.onSaveInstanceState(outState)
       }
   ```

   这种方式的弊端就是，切换后总是回到首页.

2. onSaveInstanceState() 保存Fragment

   ```
   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
   
       if (savedInstanceState != null) {
           /*获取保存的fragment  没有的话返回null*/
           homeFragment = supportFragmentManager.getFragment(savedInstanceState, HOME_FRAGMENT_KEY) as HomeFragment?
           healthFragment = supportFragmentManager.getFragment(savedInstanceState, DASHBOARD_FRAGMENT_KEY) as HealthFragment?
           personFragment = supportFragmentManager.getFragment(savedInstanceState, NOTICE_FRAGMENT_KEY) as PersonFragment?
           addToList(homeFragment)
           addToList(healthFragment)
           addToList(personFragment)
       } else {
           initFragment()
       }
    }
    
    override fun onSaveInstanceState(outState: Bundle) {
           super.onSaveInstanceState(outState)
           Timber.d("MainActivity onSaveInstanceState")
           /*fragment不为空时 保存*/if (homeFragment != null) {
               supportFragmentManager.putFragment(outState!!, HOME_FRAGMENT_KEY, homeFragment!!)
           }
           if (healthFragment != null) {
               supportFragmentManager.putFragment(outState!!, DASHBOARD_FRAGMENT_KEY, healthFragment!!)
           }
           if (personFragment != null) {
               supportFragmentManager.putFragment(outState!!, NOTICE_FRAGMENT_KEY, personFragment!!)
           }
      }
    
   ```



这种方式，可以解决问题，但是打印的日志HomeFragment还是不一样，是什么情况？

```
D/TabBottomActivity LaunchModeActivity: onCreate() taskId  4437
 D/TabBottomActivity LaunchModeActivity: initFragment() homeFragment HomeFragment{dc2914d} (5f30b0dd-a8d2-4b3c-a27f-1d160a9da58e) id=0x7f09010e}
 D/TabBottomActivity LaunchModeActivity: onStart()
 D/TabBottomActivity LaunchModeActivity: onResume()
 D/TabBottomActivity$onCreate: navigation_health
 D/TabBottomActivity LaunchModeActivity: onPause()
 D/TabBottomActivity LaunchModeActivity: onSaveInstanceState()
 D/TabBottomActivity: MainActivity onSaveInstanceState
 D/TabBottomActivity LaunchModeActivity: onStop()
 D/TabBottomActivity LaunchModeActivity: onDestroy()
 D/TabBottomActivity LaunchModeActivity: onDestroy() homeFragment HomeFragment{dc2914d} (ab276ea2-5fa2-4d79-b6a6-af7143b114e8)}
 D/TabBottomActivity LaunchModeActivity: onCreate() taskId  4437
 D/TabBottomActivity: fragmentList数量1
 D/TabBottomActivity: fragmentList数量2
 D/TabBottomActivity: fragmentList数量2
 D/TabBottomActivity LaunchModeActivity: initFragment() homeFragment HomeFragment{9139494} (5f30b0dd-a8d2-4b3c-a27f-1d160a9da58e) id=0x7f09010e}
 D/TabBottomActivity LaunchModeActivity: onStart()
 D/TabBottomActivity LaunchModeActivity: onRestoreInstanceState()
 D/TabBottomActivity LaunchModeActivity: onResume()
 D/TabBottomActivity LaunchModeActivity: onPause()
 D/TabBottomActivity LaunchModeActivity: onSaveInstanceState()
 D/TabBottomActivity: MainActivity onSaveInstanceState
 D/TabBottomActivity LaunchModeActivity: onStop()
 D/TabBottomActivity LaunchModeActivity: onDestroy()
 D/TabBottomActivity LaunchModeActivity: onDestroy() homeFragment HomeFragment{9139494} (cadcb63f-8516-4d44-8f3c-75b0c8e9f27f)}
```



https://blog.csdn.net/yuzhiqiang_1993/article/details/75014591

https://www.jianshu.com/p/78ec81b42f92



##### 下面这种方式没理解

https://www.jianshu.com/p/d9143a92ad94

https://www.jianshu.com/p/c12a98a36b2b



#### 横竖屏切换，Fragment实例复用

setRetainInstance=true

**retainInstance = true已经过期，改用ViewModel**

LifeFragment

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    retainInstance = true
}
```

```
private fun initFragment() {
    var lifeFragment = supportFragmentManager.findFragmentById(R.id.fl_content)
    if (lifeFragment == null) {
        lifeFragment = LifeFragment()
        supportFragmentManager.beginTransaction().add(R.id.fl_content, lifeFragment).commit()
    }
    Timber.d("LifeFragment $lifeFragment")
}
```

切换屏幕后 LifeFragment,ondestroy()没志行,后面的Oncreate()也没有创建.

```
 D/LifeActivity LaunchModeActivity: onCreate() taskId  599
 D/LifeActivity: LifeFragment LifeFragment{6a86311} (70133a14-6dcf-4f8b-beee-6cd31d6623ba) id=0x7f090109}
 D/LifeFragment LaunchModeFragment: onAttach(Context context)
 D/LifeFragment LaunchModeFragment: onCreate() savedInstanceState    null
 D/LifeFragment LaunchModeFragment: onCreateView()
 D/LifeFragment LaunchModeFragment: onActivityCreated()
 D/LifeFragment LaunchModeFragment: onStart()
 D/LifeFragment LaunchModeFragment: onResume()
 D/LifeFragment LaunchModeFragment: onPause() 
 D/LifeFragment LaunchModeFragment: onStop()
 D/LifeFragment LaunchModeFragment: onDestroyView()
 D/LifeFragment LaunchModeFragment: onDetach()
 D/LifeFragment LaunchModeFragment: onAttach(Context context)
 D/LifeActivity LaunchModeActivity: onCreate() taskId  599
 D/LifeActivity: LifeFragment LifeFragment{6a86311} (70133a14-6dcf-4f8b-beee-6cd31d6623ba) id=0x7f090109}
 D/LifeFragment LaunchModeFragment: onCreateView()
 D/LifeFragment LaunchModeFragment: onActivityCreated()
 D/LifeFragment LaunchModeFragment: onStart()
 D/LifeFragment LaunchModeFragment: onResume()
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



#### fragment特性

##### commitAllowingStateLoss VS  commit()区别

https://huxian99.github.io/2016/08/28/cj3qymo360000owxk9zp17alo/



##### Fragment之间传递数据

1. Fragment.setArguments()方法传递bundle

2. findFragmentById()找到tag,然后直接操作Framgent

   ```java
     public void onArticleSelected(int position) {
           ArticleFragment articleFrag = (ArticleFragment)
                   getSupportFragmentManager().findFragmentById(R.id.article_fragment);
   
           if (articleFrag != null) {
               articleFrag.updateArticleView(position);
           } else {
               ArticleFragment newFragment = new ArticleFragment();
               Bundle args = new Bundle();
               args.putInt(ArticleFragment.ARG_POSITION, position);
               newFragment.setArguments(args);
   
               FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
               transaction.replace(R.id.fragment_container, newFragment);
               transaction.addToBackStack(null);
               transaction.commit();
           }
       }
   ```

   

3. 接口回调

4. ViewModel Livedata



##### 给已经创建frament传递数据

也是直接 Fragment.refresh(),第一行代码第三版P227，也是这样做的。



##### fragment回退栈

tx.addToBackStack(null); 添加回退功能，类似Activity压栈的过程

<https://www.jianshu.com/p/fe16553ca2ca>

<https://blog.csdn.net/zhiyuan0932/article/details/52593039>

Fragment控制父Fragment展示

[Communication between nested fragments in Android](https://stackoverflow.com/questions/39491655/communication-between-nested-fragments-in-android)

https://blog.csdn.net/u011481547/article/details/71552720

Fragmet全局流程图

<https://kotlintc.com/articles/5693>

https://mp.weixin.qq.com/s/MOWdbI5IREjQP1Px-WJY1Q

https://juejin.im/post/5bcd58b6e51d45404c71d23f

https://mp.weixin.qq.com/s/_hdbMOA2TVvX4jFnMu15bg