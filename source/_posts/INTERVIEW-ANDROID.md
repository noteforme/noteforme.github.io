---

title: INTERVIEW_ANDROID
comments: true
date: 2019-01-15 10:24:37
tags: inter
categories:
---



[大佬面试经验](https://www.1point3acres.com/bbs/interview/airbnb(%E5%8C%97%E4%BA%AC)%E3%80%81%E5%BF%AB%E6%89%8B%E3%80%81%E5%B0%8F%E7%BA%A2%E4%B9%A6%E3%80%81%E7%8C%BF%E9%A2%98%E5%BA%93%E7%AD%8915%E5%AE%B6%E9%9D%A2%E7%BB%8F-mobile-ios-android-530393.html)



https://interview-q-a-1gdnkgkla15afdbe-1258598664.tcloudbaseapp.com/Android/



### Android基础知识点

##### 四大组件是什么

Activity ,Service ,BroadCastReceiver,ContentProvider

#### Activity

##### Activity各种情况下的生命周期?

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

  

##### Activity之间的通信方式

* Intent

  startActivity()或startActivityForResult(),通过Intent传递信息,需要注意，Intent对携带信息大小有限制。

- BroadcastReceiver

- 数据存取传递，sharePreference/sql/File

- Application 静态变量


##### Activity的四种启动模式对比

* Standard 

  默认启动模式，每次都重新创建一个新的Activity 。

* SingleStop

  当前Activity如果在栈顶，那么就不会创建新的Activity，会原先调用Activity的onNewIntent()
  
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



##### Android APK编译打包流程

![](https://camo.githubusercontent.com/9f62ff22761b5a960fc9547be84cd05ddec48fe4b28fbb1bd17877fb88b43ed0/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f333938353536332d636462613331396461623332643063372e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

1. AAPT（Android Asset Packaging  Tools）工具会打包应用中的资源文件，如AndroidManifest.xml、layout布局中的xml等，并将xml文件编译成二进制形式，当然assets文件夹中的文件不会被编译，图片以及raw文件夹中的资源也会保持原有的形态，需要注意的是raw文件夹中的资源也会生成资源ID。AAPT编译完成后会生成R.java文件。
2. AIDL工会将所有的aidl接口转换为java接口。
3. 所有的Java源代码、R文件、接口都会编译器编译成.class文件。
4. Dex工具会将上述产生的.class文件以及第三方库和其他class文件转化为dex（Dalvik虚拟机可执行文件）文件，dex文件最终会被打包进APK文件。
5. apkbuilder会把编译后的资源和其他资源文件同dex文件一起打入APK中。
6. 生成APK文件之后，，需要对其签名才能安装到设备上，平时测试都会使用debug keystore，当发布应用时必须使用release版的keystore对应用进行签名。
7. 如果对APK正式签名，还需要使用zipalign工具对APK进行对齐操作，这样做的好处是当应用运行时能提高速度，但是会相应的增加内存开销。

**总结：编译 --> DEX --> 打包 --> 签名和对齐**



##### ART虚拟机与Dalvik虚拟机的区别

* 什么是ART？

  ART代表Android  Runtime，其处理应用程序执行的方式完全不同于Dalvik，Dalvik是依靠一个Just-In-Time（**JIT）编译器去解释字节码**。开发者编译后的应用代码需要通过一个解释器在用户的设备上运行，这一机制并不高效，但让应用能更容易在不同硬件和架构上运行。ART则完全改变了这套做法，**在应用安装时就预编译字节码到机器语言**，这一机制叫Ahead-Of-Time（AOT）编译。在移除解释代码这一过程后，应用程序执行将更加效率。启动更快。

* ART优点：

  1. 系统性能的显著提升。
  2. 应用启动更快、运行更快、体验更流畅、触摸反馈更及时。
  3. 更长的电池续航能力
  4. 支持更低的硬件。

* ART缺点

  1. 更大的存储空间占用，可能会增加10%-20%
  2. 更长的应用安装时间



##### LaunchMode应用场景



#### Fragmment

##### Fragment生命周期管理过程遇到的坑和解决办法



##### fragment各种情况下的生命周期 Activity与Fragment之间生命周期比较

​		<u>onAttach() -> onCreate() -</u>> onCreateView() -> onActivityCreate() -> onStart() ->  onResume() -> onPause() -> onStop() -> 	 

​		onDestroyView() -> onDestroy() -> onDetach() 

##### Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用？

* Fragment发起

   Fragment onActivityResult能接收。Activity onActivityResult能接收,但是requestCode不正确。

* Activity发起

   Fragment不能接收。 Activity onActivityResult能接收。

##### fragment之间传递数据的方式？

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



#### Service

##### 请描述一下Service 的生命周期

* startService()

​	启动:  onCreate() - onCommandStart()  - onDestory()

​	继续startService : 只会执行 onCommandStart()  	

* bindService()

​	onCreate() -   onBind() - onUnbind()-onDestory()

##### service和activity怎么进行数据交互？

1. bindService()

   Service中定义接口，ServiceConnection中获取Service实例，调用响应接口

2. 注册广播传递数据说说ContentProvider、ContentResolver、ContentObserver 之间的关系
   	 把自己的程序数据提供给其他应用程序调用，提供相关的uri接口,没用过ntentObserver 之间的关系
      	 把自己的程序数据提供给其他应用程序调用，提供香港的uri接口,没用过
   
   http://wangbufan.cn/2019/09/17/Service%E5%8F%8AService%E4%B8%8EActivity%E9%80%9A%E4%BF%A1/

##### 请描述一下广播BroadcastReceiver的理解

*  广播是可以作为应用全局监听器，可以实现应用中不同组件少量数据的通信！！！，更深研究后可以多说点.

  基于消息的发布/订阅事件模型.

* 广播的分类

  1. 无序广播

  2. 有序广播

     **接收者按照优先级顺序**接收,每个接收者都**有权终止广播**,下一个就得不到.

* 广播使用的方式和场景

  app全局监听

  binder机制 https://www.jianshu.com/p/5a983578418e

  ##### BroadcastReceiver，LocalBroadcastReceiver(本地广播) 区别

  BroadcastReceiver：针对应用间，系统和应用间通信。

  LocalBroadcastReceiver： 只有自己应用内部才能收到,效率更高.



#### Dialog

##### AlertDialog,popupWindow,Toast区别 ？

Android是不允许Activity或Dialog凭空出现的,而Dialog则必须在一个Activity上面弹出

* AlertDialog 

     拦截了屏幕上所有的TouchK/key

* PopupWindow 

    仅仅拦截自身区域touch/key

    需要Activity类型的Context启动

* Toast

    可以研究下 两者最根本的区别在于有没有新建一个 window，PopupWindow 没有新建，而是通过 WMS 将 View 加到 DecorView；Dialog 是新建了一个 window (PhoneWindow)，相当于走了一遍 Activity 中创建 window 的流程

    https://www.jianshu.com/p/aed496937bd2

##### Application 和 Activity 的 Context 对象的区别

ApplicatioContext ：

​	应用生命周期一样长,长生命周期对象就用ApplicationContext

Activity的Context：

​	 当前Activity的生命周期,和UI相关的都用Activity为Context来处理

https://www.jianshu.com/p/e215c90a460e



#####  Context的理解？

​	Context是维持Android程序中各组件能够正常工作的一个核心功能类





#### Anim

##### Android属性动画特性

Android动画框架实现原理

可以改变对象的属性。还需要说说什么吗?



##### 如何导入外部数据库?

把数据库文件防盗asserts目录下，然后写入databases目录下面, 然后通过数据库容器装载里面数据库里面的数据。

https://blog.csdn.net/chaoyu168/article/details/50467913



#####  插值器 估值器区别

​	 插值器: （`Interpolator`）决定 值 的变化模式（匀速、加速）

​	 估值器 : 	(`TypeEvaluator`)决定 值 的具体变化数值



##### 谈谈对接口与回调的理解

* 理解: A发送消息给B,B处理完后高速A处理结果.

* 实现: 一般而言，处理消息的类是唯一的，发送消息的类却是各种各样的，将回调方法做成一个接口，不同的发送者实现该接口，并且把自己的接口实现类的对象在发送消息时，传递给消息处理者。

注册之后不马上执行，而是某个时机再触发执行。

##### 回调的原理 写一个回调demo

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



#### VIEW相关

##### 如何优化自定义View



##### android view绘制机制和加载过程，请详细说下整个流程

每个Activity包含一个Window对象，Android中window对象由PhoneWindow实现，PhoneWindow将一个DecorView设置为整个应用窗口的根View,DecorView作为窗口界面的顶层视图，封装了窗口操作的通用方法，DecorView将要显示的具体内容显示在PhoneWindow上，这里所有的View监听事件通过WindowMangerService来接收,通过Activity对象来回调相应的onCLicklistener.显示是将屏幕分成两部分，一个TitleView，另一个是ContentView.

 Measure

如果是原始的 View,通过measure方法就完成了测量过程,如果是ViewGroup,除了完成自己的测量外，还需要遍历所有的子View,各个子元素再去递归执行这个流程.

Layout 

Draw



##### MeasureSpeck的意义，怎么计算MeasureSpec

##### wrap content 和MATCH_PARENT的测量方式

WRAP_CONTENT : 最大模式，大小不定，但是不能超过窗口的大小. specMode是AT_MOST模式,这种模式下，它的宽，高等于specSize,								 这种情况下specSize是parentSize,而parentSize是父容器目前可以使用的大小,也就是父容器剩余的空间大小.

MATCH_PARENT: 精确模式，大小就是窗口大小.

https://noteforme.github.io/2017/11/12/View_OVER/



##### LayoutParams是是什么



##### 介绍下SurfaceView

* SurfaceView使用双缓冲技术缓解，页面绘制频繁引起的卡顿。

* SurfaceView可以在子线程更新 UI,不会阻塞主线程，提高响应速度。

  https://noteforme.github.io/2019/11/05/SurfaceView/
  
  SurfaceView在更新视图时用到了两张 Canvas，可以先创建一个临时的Canvas对象，将图像都绘制到这个临时的Canvas对象中，绘制完成之后再将这个临时Canvas对象中的内容(也就是一个Bitmap)，通过drawBitmap()方法绘制到onDraw()方法中的canvas对象中。

#####  RecycleView的使用

​	https://noteforme.github.io/2017/07/17/RecyclerView/

##### webview安全问题  

WebView漏洞的根源在于强制其访问攻击者控制的网页。网页中含有攻击者可以控制的JS,因此可能钓鱼，窃取私有文件，甚至是 RCE,带来比较大的危害。

下面主要是4.4系统以上的机型

#####  Webview密码明文存储漏洞

WebView默认开启密码保存功能mWebView.setSavePassword(true),如果未关闭，用户输入密码时，会弹出提示框，询问用户是否保存密码，如果选是，密码会明文保存到 /data/data/com.package.name/databases/webview.db

##### WebView域控制不严格漏洞

setAllowFileAccess(true) : 窃取APP任意目录下的私有文件

​	setAllowUniversalAccessFromFileURLs : 允许通过file域url中的 javascript访问其他的源。

https://noteforme.github.io/2017/09/01/WebView/

##### webview内存泄漏 Leakcanary 验证?

​	android 5.0以下有内存泄漏问题

https://juejin.cn/post/6901487965562732551



viewstub延迟加载原理

```java
   // 设置 ViewStub 不进行绘制
     setWillNotDraw(true);
     
     
private void replaceSelfWithView(View view, ViewGroup parent) {
    final int index = parent.indexOfChild(this);
    // 把 ViewStub 从控件层级中移除。
    parent.removeViewInLayout(this);

    // 把新创建的 View 对象加入控件层级结构中，并且位于 ViewStub 的位置，
    // 并且在这个过程中，会使用 ViewStub 的布局参数，例如宽高等。
    final ViewGroup.LayoutParams layoutParams = getLayoutParams();
    if (layoutParams != null) {
        parent.addView(view, index, layoutParams);
    } else {
        parent.addView(view, index);
    }
}

```

​	https://juejin.cn/post/6844903799337779214

##### overdraw过度绘制优化方法

​	移除默认和不必要背景

​	https://jaeger.itscoder.com/android/2016/09/29/android-performance-overdraw.html

​	https://www.jianshu.com/p/9e095bacf44a



- View刷新机制
- View绘制流程
- 自定义控件原理
- 如何取消AsyncTask？
- 为什么不能在子线程更新UI？
- Requestlayout，onlayout，onDraw，DrawChild区别与联系
- invalidate和postInvalidate的区别及使用
- Activity-Window-View三者的差别
- 自定义View如何考虑机型适配
- 自定义View的事件
- 封装View的时候怎么知道view的大小



#### 事件分发机制

是否解决过事件冲突问题，怎么解决的。

问题: https://juejin.cn/post/6922300686638153736

https://noteforme.github.io/categories/VIEW/

https://www.bilibili.com/video/BV1754y1H7jT



请描述一下View事件传递分发机制

- Touch事件传递流程
- 事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
- View和ViewGroup分别有哪些事件分发相关的回调方法

  



#### 我的面试问题

##### activity被销毁后重建，怎么获取fragment

​	findFragmentByTag

​	https://www.wanandroid.com/wenda/show/11077



##### activity和service通信

1. 通过bindService

2. 通过广播

   https://www.jianshu.com/p/6040dfa83594

##### activity销毁线程会不会消失

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

##### activity每个方法处理的区别

1. onCreate():  Activity创建的时候调用，绑定数据。

2. onStart() : 当Activity对用户变得可见的时候调用.

3. onResume() : activity开始和用户交互的时候调用，这时候activity处于栈顶，伴随着用户的输入.

4. onPause() : 当activity失去前台状态，开始进入stopped/hidden or destroyed状态，不能再获取焦点，此时activity对用户可见，但是更新的UI的操作要快。

5. onStop()  :  当activity不再可见，可能是新的activity来到栈顶，或者当前activity正在被destroy.

6. onDestory(): 当前activity正在离开。

   

##### activity fragment传递数据方式

*  ViewModel

* onTach() 回调

  

##### 单例模式怎么理解的

  创建唯一的对象 

* res/raw和assets 三者目录下的文件在打包后原封不动的保存在apk包中，不会被编译成二进制。
* Res/raw文件会被映射到R.java文件中，访问的时候直接使用资源 R.id.filename;
* res/raw不可以有目录结构，而asserts则可以有目录结构，也就是asserts目录下可以建立文件夹.

https://www.jianshu.com/p/4c8bcb8c3717



##### 截获通知

新建一个服务MessageNotificationService实现 NotificationListenerService

##### final特性

1. final类不能被继承，没有子类
2. 方法不能被子类的方法重写，但可以被继承
3. 表示常量，只能被赋值一次，赋值后不再改变。



##### android多线程怎么处理的 ，两个子线程间怎么通讯

​	Android主线程和子线程之间的通信是通过消息循环机制，主线程中的handler把子线程的 message发送给主线程的Looper，那么子线程是如何通信的， 可以把looper绑定到子线程中，调用Looper.prepare()为改子线程生成Looper,然后调用Looper.loop()启动消息队列，并且在该子线程中创建一个Handler,在另一个子线程调用handler发送消息。这样实现通信.

```kotlin
val threadA = ThreadA()
val threadB = ThreadB()
Thread(threadA).start()
if (threadA.getHandler() == null) {
    Thread.sleep(1000)
    handler = threadA.getHandler()
}
Thread(threadB).start()

class ThreadA : Runnable {
        var mHandler: Handler? = null


        fun getHandler(): Handler? {
            return mHandler
        }

        override fun run() {
            Looper.prepare()
            mHandler = object : Handler(Looper.myLooper()!!) {
                override fun handleMessage(msg: Message) {
                    super.handleMessage(msg)
                    Timber.d("线程A: 线程B发过来消息了-- ${msg.obj} ")
                }
            }
            Looper.loop()
        }
    }


    inner class ThreadB : Runnable {
        override fun run() {
            val message = Message.obtain()
            message.what = 1
            message.obj = "线程B 发送消息" + System.currentTimeMillis()
            handler?.sendMessage(message)
        }
    }
```

https://blog.csdn.net/pbm863521/article/details/103493708



##### 防止应用被被杀死,怎么保证service不被杀死

http://www.52im.net/thread-2881-1-1.html

http://www.52im.net/thread-2893-1-1.html



##### 怎么让线程有序

##### 两个线程，想让来的一个插队怎么弄

​	Join

批量网络请求

子线程创建Handler

context对象互相引用，对象回收 Android界面怎么回收的 生命周期 

内存溢出和内存泄露的区别，oom是怎么处理的 ， ANR怎么避免

java内存回收机制 android管理机制 怎么处理内存泄露

怎么做性能优化

反射机制

broadcastReciever和 handler区别

后台图片更改，前台怎么处理

图片加载库,图片加载方法

图片加载框架怎么处理oom问题

java Android加载机制

Android加载动态库

AndroidManifest权限是怎么获取的，封装权限管理，为什么需要权限分组.

listview图片 缓存，listview怎么优化

viewpager listview处理滑动冲突 

list遍历删除

fragment tag

sercice ALDL (后面再弄)

Hashmap实现原理  实现有序

android事件分发机制，请详细说下整个流程

android四大组件的加载过程

提高sqlite的查询效率

冒泡排序，插入排序 

### Android源码相关分析

#####  Handler机制和底层实现

​	https://noteforme.github.io/2017/08/21/Handler/

##### RecycleView

​	https://noteforme.github.io/2017/07/17/RecyclerView/



##### Binder通信原理与机制

https://blog.csdn.net/Android_SE/article/details/103898581

https://www.bilibili.com/video/BV1Ko4y117Ca?p=74



##### JetPack

https://noteforme.github.io/categories/Jetpack/



- Android各个版本API的区别

- 描述一次网络请求的流程

- Bitmap对象的理解

- ActivityThread，AMS，WMS的工作原理

- SpareArray原理

- AndroidService与Activity之间通信的几种方式

- IntentService原理及作用是什么？

- 说说Activity、Intent、Service 是什么关系

- ApplicationContext和ActivityContext的区别

- SP是进程同步的吗?有什么方法做到同步？

- 谈谈多线程在Android中的使用

- 进程和 Application 的生命周期

- AsyncTask机制

- AsyncTask原理及不足

- AndroidManifest的作用与理解

  

### 性能优化

https://noteforme.github.io/2017/08/16/PerformancePatterns/

https://noteforme.github.io/2018/02/09/LeakMemory/

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
- 如何通过广播拦截和abort一条短信？
- 广播引起anr的时间限制是多少？
- 计算一个view的嵌套层级
- Activity栈
- Android线程有没有上限？
- 线程池有没有上限？
- Android为什么引入Parcelable？
- 有没有尝试简化Parcelable的使用？

（四）开发中常见的一些问题

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

- ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？

- 点击事件被拦截，但是想传到下面的View，如何操作？

- 微信上消息小红点的原理

- CAS介绍（这是阿里巴巴的面试题，我不是很了解，可以参考博客: [CAS简介](http://blog.csdn.net/jly4758/article/details/46673835)）

  

https://github.com/android-exchange/Android-Interview

Kotin面试题

http://www.youkmi.cn/2019/10/27/kotlin-ti-mu-zheng-li/

https://www.jianshu.com/p/45866c8415c8



怎么看源码 

https://www.bilibili.com/video/BV1d54y1h768



面经

https://blog.csdn.net/sjy0118/article/details/112759112

https://www.kaelli.com/43.html

https://www.jianshu.com/p/058c54948ca5

https://juejin.cn/post/6888222422760488974#heading-51

https://github.com/Omooo/Android_QA
