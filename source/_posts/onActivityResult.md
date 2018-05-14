---
title: onActivityResult
comments: true
date: 2017-09-26 14:01:53
tags:
categories: ANDROID

---


#  单个页面跳转

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


# Fragment接收onActivityResult

**必须使用Fragment 的 startActivityForResult()**

```
 public static void startForResult(Activity activity, Fragment fragment) {
        Intent intent = new Intent(activity, LoginActivity.class);
        fragment.startActivityForResult(intent, REQUEST_CODE_MINE_FRAGMENT);
    }

```

setResult(RESULT_OK, result); ,finish()后 onActivityResult 返回 resultCode 

 


# 跳转多个页面 回来

中间页面不能finish，也需要在onActivityResult方法内部 起到桥梁作用，回传数据

http://blog.csdn.net/lanyachuanshu/article/details/52172143
https://developer.android.com/training/basics/intents/result.html

http://45.77.222.97:3000/root/MineUtils/src/master/app/src/main/java/com/jonzhou/mineutils/result/FirstResultActivity.java