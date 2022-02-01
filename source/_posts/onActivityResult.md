---
title: onActivityResult
comments: true
date: 2017-09-26 14:01:53
tags:
categories: ANDROID

---

#### 传统方式

###### 单个页面跳转

 主要有三个方法实现
*  startActivityForResult(@RequiresPermission Intent intent, int requestCode)
*  setResult(int resultCode, Intent data)
*  onActivityResult(int requestCode, int resultCode, Intent data)

下面是简单例子
跳转MainActivity

目标SecondAcitivty


其中数据传递很简单,特别注意requestCode，resultCode不同的区分的区别

* requestCode：假设  MainActivity　button1 button2到SecondAcitivty,回来的在onActivityResult就需要
         判断是哪个button点击的

* resultCode: 假设MainActivity　button1跳转到SecondAcitivty,  button2到ThirdActiivty,回来后onActivityResult
         就需要判断是哪个Activity回来的
     参考:http://blog.csdn.net/jiangwei0910410003/article/details/16983049



*注意*:onActivityResult方法用的算比较普遍，刚好有个困惑，回传数据时`intent.putExtra("text",122);`  使用 `  String couponId = data.getStringExtra("text");` 接收时为null,里面跳转和回传,存的值是int类型，获取string类型

###### Fragment接收onActivityResult

*必须使用Fragment 的 startActivityForResult()**

```java
 public static void startForResult(Activity activity, Fragment fragment) {
        Intent intent = new Intent(activity, LoginActivity.class);
        fragment.startActivityForResult(intent, REQUEST_CODE_MINE_FRAGMENT);
  }

```

setResult(RESULT_OK, result); ,finish()后 onActivityResult 返回 resultCode 



#### Activity嵌套Fragment,他们各自发起startActivityForResult发起数据请求

##### 结论

 ![](onActivityResult/20161119124707067.jpg)

* Fragment发起

   Fragment onActivityResult能接收。Activity onActivityResult能接收,但是requestCode不正确。

* Activity发起

   Fragment不能接收。 Activity onActivityResult能接收。

##### 代码验证

FirstResultActivity

```kotlin
  val requestCode = 1000
    override fun initView() {
        super.initView()
        val intent = Intent(this, SecondResultActivity::class.java)
        findViewById<View>(R.id.bt_activity).setOnClickListener {
            startActivityForResult(intent, requestCode)
            Timber.i("startActivityForResult $requestCode")
        }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        Timber.d("onActivityResult resultCode $requestCode resultCode $resultCode  data  ${data?.getStringExtra(SecondResultActivity.sData)}")
    }

    private fun initFragment() {
        val ftn = supportFragmentManager.beginTransaction().replace(R.id.fl_content, newInstance("数据"))
        ftn.commit()
    }
```

ResultOkFragment

```kotlin
    private val requestCode: Int = 2000
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        val intent = Intent(context, SecondResultActivity::class.java)

        bt_fragment.setOnClickListener {
            startActivityForResult(intent, requestCode)
            Timber.d("startActivityForResult $requestCode")
        }
    }


    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        Timber.d("onActivityResult resultCode $requestCode resultCode $resultCode  data 			${data?.getStringExtra(SecondResultActivity.sData)}")
    }


```

SecondResultActivity

```java
setResult(21, new Intent().putExtra(sData, "我要回调啦!!!"));
finish();
```



* Activity发起

  ```
  I/FirstResultActivity$initView: startActivityForResult 1000
  D/FirstResultActivity: onActivityResult resultCode 1000 resultCode 21  data  我要回调啦!!!
  ```

* Fragment发起

  ```
  D/ResultOkFragment$onActivityCreated: startActivityForResult 2000
  D/ResultOkFragment: onActivityResult resultCode 2000 resultCode 21  data 我要回调啦!!!
  D/FirstResultActivity: onActivityResult resultCode 329680 resultCode 21  data  我要回调啦!!!
  ```

  

#### 跳转多个页面 回来

中间页面不能finish，也需要在onActivityResult方法内部 起到桥梁作用，回传数据

http://blog.csdn.net/lanyachuanshu/article/details/52172143
https://developer.android.com/training/basics/intents/result.html

http://45.77.222.97:3000/root/MineUtils/src/master/app/src/main/java/com/jonzhou/mineutils/result/FirstResultActivity.java

https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650826252&idx=1&sn=9ec620d630706c9f8de328e87bb8a0f1&chksm=80b7b292b

#####   ActivityResultContract新方式

https://mp.weixin.qq.com/s/o2_2DeAIcJGYSvm2WlhUxg



