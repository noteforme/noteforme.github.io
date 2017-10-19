---
title: onActivityResult
comments: true
date: 2017-09-26 14:01:53
tags:
categories: ANDROID

---


# 单个页面跳转

使用

## 跳转MainActivity

```
 Intent intent = new Intent(this,SecondActivity.class);
        startActivityForResult(intent,1);


  String dd = data.getStringExtra("dd");
        System.out.println(dd);

```

## 目标SecondAcitivty

```

  Intent intent = new Intent();
        intent.putExtra("dd","fafjjf");
        setResult(1,intent);
        finish();

```



onActivityResult方法用的算比较普遍，刚好有个困惑，回传数据时`intent.putExtra("text",122);`  
使用 `  String couponId = data.getStringExtra("text");` 接收时为null,里面跳转和回传的
内部机制是怎么样

存的值是int类型，获取string类型

# 跳转多个页面 回来

中间页面不能finish，也需要在onActivityResult方法内部 起到桥梁作用，回传数据

http://blog.csdn.net/lanyachuanshu/article/details/52172143

