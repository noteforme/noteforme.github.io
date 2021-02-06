---
title: INTERVIEW_ANDROID
comments: true
date: 2021-01-15 10:24:37
tags: resume
categories: ANDROID
---

### 一）Android基础知识点

#### 四大组件是什么

Activity ,Service ,BroadCastReceiver,ContentProvider

#### Activity各种情况下的生命周期

* 先启动A 再跳转B

  A_onCreate()-> A_onStart()->A_onResume-> A_onPause()-> B_onCreate() -> B_onStart() -> B_resume -> A_onSaveInstanceState()->A_onStop()
  
* 弹出Dialog

  不调用任何生命周期,所以Activity上有Dialog的时候按Home键时的生命周期,有没有Dialog都一样的。

* 横竖屏切换的时候，Activity 各种情况下的生命周期

  

  1. Activity状态保存于恢复 (什么都不设置)

     ![](INTERVIEW-ANDROID/Screen Shot 2021-01-24 at 3.37.01 PM.png)

  2. 设置`android:screenOrientation="portrait"` 不会旋转

  3. Android 8.0 设置`android:configChanges="orientation|keyboardHidden|screenSize"` 

     会发生旋转，生命周期不发生变化，只是会调用 `onConfigurationChanged()` 

     https://blog.csdn.net/qq_36713816/article/details/80538467

     

* 前台切换到后台，然后再回到前台，Activity生命周期回调方法。

  前台切换到后台: A_onCreate()-> A_onStart()->A_onResume-> A_onPause()->A_onSaveInstanceState()-> A_onStop()

  再回到前台: A_onRestart() ->A_onStart()-> A_onResume()

  

#### Activity之间的通信方式

* Intent

  startActivity()或startActivityForResult(),通过Intent传递信息,需要注意，Intent对携带信息大小有限制。

- BroadcastReceiver

- 数据存取传递，sharePreference/sql/File

- Application 静态变量

  



#### Activity的四种启动模式对比

* Standard 

  默认启动模式，每次都重新创建一个新的Activity 。

* SingleStop

  当前Activity如果再栈顶，那么就不会创建新的Activity，会原先调用Activity的onNewIntent()
  
* SingleTask

  当前任务栈已经有Activity实例，就不会再创建了，会调用 onNewIntent().

* SingleInstance

  单一实例模式，整个手机操作系统里面只有一个实例存在。不同的应用去打开这个activity  共享公用的同一个activity。他会运行在自己单独，独立的任务栈里面，并且任务栈里面只有他一个实例存在。应用场景：呼叫来电界面。这种模式的使用情况比较罕见，在Launcher中可能使用。或者你确定你需要使Activity只有一个实例。 
   可以得出以下结论： 
   \1. 以singleInstance模式启动的Activity具有全局唯一性，即整个系统中只会存在一个这样的实例。 
   \2. 以singleInstance模式启动的Activity在整个系统中是单例的，如果在启动这样的Activiyt时，已经存在了一个实例，那么会把它所在的任务调度到前台，重用这个实例。 
   \3. 以singleInstance模式启动的Activity具有独占性，即它会独自占用一个任务，被他开启的任何activity都会运行在其他任务中。 
   \4. 被singleInstance模式的Activity开启的其他activity，能够在新的任务中启动，但不一定开启新的任务，也可能在已有的一个任务中开启。
  
  https://blog.csdn.net/zivensonice/article/details/51569502
  
  https://ayusch.com/android-launch-modes-explained/
  
  https://noteforme.github.io/2021/01/16/Activity/
  
* 为什么 application.startActivity 要设置NEW_TASK

  如果Activity是由一个已经启动的Activity发起的，那么把它放在这个已经启动的任务栈是合理的，Application本来没有任务栈，那么就新创建一个放起来.

  https://www.wanandroid.com/wenda/show/8697

任务栈的底层原理



#### fragment各种情况下的生命周期 Activity与Fragment之间生命周期比较

1.<u>onAttach() -> onCreate() -</u>> onCreateView() -> onActivityCreate() -> onStart() ->  onResume() -> onPause() -> onStop() -> 	 

​	onDestroyView() -> onDestroy() -> onDetach() 

#### Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用？

* Fragment发起

   Fragment onActivityResult能接收。Activity onActivityResult能接收,但是requestCode不正确。

* Activity发起

   Fragment不能接收。 Activity onActivityResult能接收。

#### fragment之间传递数据的方式？

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

4. ViewModel Livedata ?

   

#### 请描述一下Service 的生命周期

##### startService()

​	启动:  onCreate() - onCommandStart()  - onDestory()

​	继续startService : 只会执行 onCommandStart()  	

##### bindService()

​	onCreate() -   onBind() - onUnbind()-onDestory()

#### service和activity怎么进行数据交互？

1. bindService()

   Service中定义接口，ServiceConnection中获取Service实例，调用响应接口

2. 注册广播传递数据

   http://wangbufan.cn/2019/09/17/Service%E5%8F%8AService%E4%B8%8EActivity%E9%80%9A%E4%BF%A1/

#### 说说ContentProvider、ContentResolver、ContentObserver 之间的关系

​	 把自己的程序数据提供给其他应用程序调用，提供香港的uri接口,没用过

#### 请描述一下广播BroadcastReceiver的理解

​	广播是可以作为应用全局监听器，可以实现应用中不同组件少量数据的通信！！！，更深研究后可以多说点.

#### 广播的分类

1. 无序广播
2. 有序广播

#### 广播使用的方式和场景

app全局监听

binder机制 https://www.jianshu.com/p/5a983578418e

#### BroadcastReceiver，LocalBroadcastReceiver(本地广播) 区别

BroadcastReceiver：针对应用间，系统和应用间通信。

 LocalBroadcastReceiver： 只有自己应用内部才能收到,效率更高.

#### AlertDialog,popupWindow,Toast区别 ？

* AlertDialog : 拦截了屏幕上所有的TouchK/key

* PopupWindow 

    仅仅拦截自身区域touch/key

    需要Activity类型的Context启动

* Toast

* 可以研究下 两者最根本的区别在于有没有新建一个 window，PopupWindow 没有新建，而是通过 WMS 将 View 加到 DecorView；Dialog 是新建了一个 window (PhoneWindow)，相当于走了一遍 Activity 中创建 window 的流程

  https://www.jianshu.com/p/aed496937bd2

#### Application 和 Activity 的 Context 对象的区别

ApplicatioContext ：

​	应用生命周期一样长,长生命周期对象就用ApplicationContext

Activity的Context：

​	 当前Activity的生命周期,和UI相关的都用Activity为Context来处理

https://www.jianshu.com/p/e215c90a460e

#### Android属性动画特性

可以改变对象的属性。还需要说说什么吗?

#### 如何导入外部数据库?

把数据库文件防盗asserts目录下，然后写入databases目录下面, 然后通过数据库容器装载里面数据库里面的数据。

https://blog.csdn.net/chaoyu168/article/details/50467913

#### LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。

1. LinearLayout ： 水平或垂直位置按照顺序来排列子元素.
2. RelativeLayout:  有相对的参照物，进行布局.
3. FrameLayout: 后面添加的会覆盖前面的布局。

#### 谈谈对接口与回调的理解

* 理解: A发送消息给B,B处理完后高速A处理结果.

* 实现: 一般而言，处理消息的类是唯一的，发送消息的类却是各种各样的，将回调方法做成一个接口，不同的发送者实现该接口，并且把自己的接口实现类的对象在发送消息时，传递给消息处理者。

注册之后不马上执行，而是某个时机再触发执行。

#### 回调的原理

#### 写一个回调demo

```java
public class MyTest {
    public static void main(String[] args) {
        ProcessClick process = new ProcessClick(new OnClickListener() {
            @Override
            public void onClick() {
                System.out.println("已经点击");
            }
        });
        process.click();
    }
}


interface OnClickListener{
    void  onClick();
}

class  ProcessClick {
    OnClickListener listener;

    public ProcessClick(OnClickListener listener) {
        this.listener = listener;
    }

    void click(){
        listener.onClick();
    }
}

```

https://developer.aliyun.com/article/614769

#### 介绍下SurfaceView

* SurfaceView使用双缓冲技术缓解，页面绘制频繁引起的卡顿。

* SurfaceView可以在子线程更新 UI,不会阻塞主线程，提高响应速度。

  https://noteforme.github.io/2019/11/05/SurfaceView/

#### ? RecycleView的使用



####  插值器 估值器区别

 插值器: （`Interpolator`）决定 值 的变化模式（匀速、加速）

估值器 : 	(`TypeEvaluator`)决定 值 的具体变化数值



#### 我的面试问题

activity销毁后重新创建，怎么获取fragment

#### activity和service通信

1. 通过bindService

2. 通过广播

   https://www.jianshu.com/p/6040dfa83594

#### activity销毁线程会不会消失

不会,  可以这样处理

```java
 /**
   * 静态内部类将不会再隐式的持有外部类的引用，所以在配置改变时，你的Activity的实例在也不会
   * 出现内存泄露
   */
  private static class MyThread extends Thread {
    private boolean mRunning = false;

    @Override
    public void run() {
      mRunning = true;
      while (mRunning) {
        SystemClock.sleep(1000);
      }
    }

    public void close() {
      mRunning = false;
    }
  }
```

activity每个方法处理的区别

activity fragment传递数据方式

android多线程怎么处理的 ，两个子线程间怎么通讯

android广播怎么处理的

动画  怎么处理,

写一个方法可以调用本类的方法

为什么非静态方法可以直接被调用

单例模式怎么理解的

写一个网络请求

res里面的图片会不会转换成二进制格式

线程怎么管理

怎么让线程有序 :加入队列

两个线程池，想让来的一个插队怎么弄

批量网络请求

防止应用被被杀死,怎么保证service不被杀死

final特性

Android启动模式 singleinstance. 和singletop区别应用场景

wrap content 和martch content的绘制方法

自定义控件

context对象互相引用，对象回收 Android界面怎么回收的 生命周期 a界面到b的onresume在a的onstop之前还是之后

android怎么对内存进行分析

内存溢出和内存泄露的区别，oom是怎么处理的 ， ANR怎么避免

java内存回收机制 android管理机制 怎么处理内存泄露

后台图片更改，前台怎么处理

反射机制

除了handler还有什么能传递数据

子线程创建Handler

handler启动一个线程和asnnaltask区别 单元测试

broadcastreciever和 handler区别

图片加载库,图片加载方法

图片加载框架怎么处理oom问题

怎么做性能优化

截获通知

app怎么接收不到推送消息怎么办(2017.8.17)

java Android加载机制

Android加载动态库

AndroidManifest权限是怎么获取的，封装权限管理，为什么需要权限分组.

listview内回收图片

listview图片 缓存，listview怎么优化

viewpager listview处理滑动冲突 



webview内泄漏,和安全问题  

多线程死锁(图片网络请求会出现的问题

viewstub延迟加载

listview recycleview的选择

overdraw过度绘制方法https://www.jianshu.com/p/9e095bacf44a

opengl

单元测试覆盖率

list遍历删除

fragment tag

 **sercice ALDL** (后面再弄)

hashmap实现原理  实现有序

android事件分发机制，请详细说下整个流程

android view绘制机制和加载过程，请详细说下整个流程

android四大组件的加载过程

提高sqlite的查询效率

冒泡排序，插入排序 

contentprovider





##### （二）Android源码相关分析

- Android动画框架实现原理
- Android各个版本API的区别
- Requestlayout，onlayout，onDraw，DrawChild区别与联系
- invalidate和postInvalidate的区别及使用
- Activity-Window-View三者的差别
- 谈谈对Volley的理解
- 如何优化自定义View
- 低版本SDK如何实现高版本api？
- 描述一次网络请求的流程
- HttpUrlConnection 和 okhttp关系
- Bitmap对象的理解
- looper架构
- ActivityThread，AMS，WMS的工作原理
- 自定义View如何考虑机型适配
- 自定义View的事件
- AstncTask+HttpClient 与 AsyncHttpClient有什么区别？
- LaunchMode应用场景
- AsyncTask 如何使用?
- SpareArray原理
- 请介绍下ContentProvider 是如何实现数据共享的？
- AndroidService与Activity之间通信的几种方式
- IntentService原理及作用是什么？
- 说说Activity、Intent、Service 是什么关系
- ApplicationContext和ActivityContext的区别
- SP是进程同步的吗?有什么方法做到同步？
- 谈谈多线程在Android中的使用
- 进程和 Application 的生命周期
- 封装View的时候怎么知道view的大小
- RecycleView原理
- AndroidManifest的作用与理解

##### （三）常见的一些原理性问题

- Handler机制和底层实现
- Handler、Thread和HandlerThread的差别
- handler发消息给子线程，looper怎么启动？
- 关于Handler，在任何地方new Handler 都是什么线程下?
- ThreadLocal原理，实现及如何保证Local属性？
- 请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系
- 请描述一下View事件传递分发机制
- Touch事件传递流程
- 事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
- View和ViewGroup分别有哪些事件分发相关的回调方法
- View刷新机制
- View绘制流程
- 自定义控件原理
- 自定义View如何提供获取View属性的接口？
- Android代码中实现WAP方式联网
- AsyncTask机制
- AsyncTask原理及不足
- 如何取消AsyncTask？
- 为什么不能在子线程更新UI？
- ANR产生的原因是什么？
- ANR定位和修正
- oom是什么？
- 什么情况导致oom？
- 有什么解决方法可以避免OOM？
- Oom 是否可以try catch？为什么？
- 内存泄漏是什么？
- 什么情况导致内存泄漏？
- 如何防止线程的内存泄漏？
- 内存泄露场的解决方法
- 内存泄漏和内存溢出区别？
- LruCache默认缓存大小
- ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)
- 如何通过广播拦截和abort一条短信？
- 广播是否可以请求网络？
- 广播引起anr的时间限制是多少？
- 计算一个view的嵌套层级
- Activity栈
- Android线程有没有上限？
- 线程池有没有上限？
- ListView重用的是什么？
- Android为什么引入Parcelable？
- 有没有尝试简化Parcelable的使用？

##### （四）开发中常见的一些问题

- ListView 中图片错位的问题是如何产生的?
- 混合开发有了解吗？
- 知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，H5，小程序，WPA等。做Android的了解一些前端js等还是很有好处的)；
- 屏幕适配的处理技巧都有哪些?
- 服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？
- 动态布局的理解
- 怎么去除重复代码？
- 画出 Android 的大体架构图
- Recycleview和ListView的区别
- ListView图片加载错乱的原理和解决方案
- 动态权限适配方案，权限组的概念
- Android系统为什么会设计ContentProvider？
- 下拉状态栏是不是影响activity的生命周期
- 如果在onStop的时候做了网络请求，onResume的时候怎么恢复？
- Bitmap 使用时候注意什么？
- Bitmap的recycler()
- Android中开启摄像头的主要步骤
- ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？
- 点击事件被拦截，但是想传到下面的View，如何操作？
- 微信主页面的实现方式
- 微信上消息小红点的原理
- CAS介绍（这是阿里巴巴的面试题，我不是很了解，可以参考博客: [CAS简介](http://blog.csdn.net/jly4758/article/details/46673835)）



https://github.com/android-exchange/Android-Interview



