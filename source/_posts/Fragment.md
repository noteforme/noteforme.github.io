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





#### life

https://developer.android.com/guide/fragments/lifecycle

* onViewCreated(View view, Bundle savedInstanceState)

Called immediately after `onCreateView(android.view.LayoutInflater, android.view.ViewGroup, android.os.Bundle)` has returned, but before any saved state has been restored in to the view.



![](Fragment/fragment-view-lifecycle.png)



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



##### Fragment之前传递数据

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





##### fragment回退栈

tx.addToBackStack(null); 添加回退功能，类似Activity压栈的过程

<https://www.jianshu.com/p/fe16553ca2ca>

<https://blog.csdn.net/zhiyuan0932/article/details/52593039>

Fragment控制父Fragment展示

https://blog.csdn.net/u011481547/article/details/71552720

Fragmet全局流程图

<https://kotlintc.com/articles/5693>



模拟系统内存不足

<http://yifeng.studio/2016/12/15/android-fragment-attentions/>

[Communication between nested fragments in Android](https://stackoverflow.com/questions/39491655/communication-between-nested-fragments-in-android)



Fragment   viewpager

https://mp.weixin.qq.com/s/MOWdbI5IREjQP1Px-WJY1Q

https://juejin.im/post/5bcd58b6e51d45404c71d23f



https://mp.weixin.qq.com/s/_hdbMOA2TVvX4jFnMu15bg