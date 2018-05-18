---
title: AndroidTesting_02
comments: true
date: 2018-01-22 18:04:40
tags:
categories: ANDROID
---



## Mockito

 作用就是创建一个虚假得对象，在测试环境代替真实对象
 	

- 验证这个对象的某些方法的调用情况，调用了多少次，参数是什么
- 指定这个对象的某些方法的行为，返回特定的值，或者是执行特定的动作

> 注意：Mockito不支持mock匿名类、final类、静态方法和private方法。

 示例：https://static.javadoc.io/org.mockito/mockito-core/2.16.0/org/mockito/Mockito.html

#### mock与spy的区别

```
	  @Test
 public void mockDifSpy() throws Exception {
    User userMock = mock(User.class);
    userMock.setNumber(2);
    int num = userMock.getNumber();
    System.out.println("mock返回  "+num);

    User userSpy = spy(User.class);
    userSpy.setNumber(5);
    System.out.println("spy返回   "+userSpy.getNumber());
 }
```

 mock返回  0
 spy返回   5

mock  :  如果不指定mock方法的特定行为，一个mock对象的所有非void方法都将返回默认值：int、long类型方法将返回0，boolean方法将返回false，对象方法将返回null等等；而void方法将什么都不做。

spy :  	spy对象的方法默认调用真实的逻辑，mock对象的方法默认什么都不做，或直接返回默认值。

 参考　https://www.jianshu.com/p/0a8bbfe6cba2



实例讲解

 http://blog.csdn.net/qq_17766199/article/details/78450007
 小创作 https://github.com/ChrisZou/android-unit-testing-tutorial
https://github.com/qingmei2/Sample_AndroidTest

一个Android项目搞定所有主流架 mvp生成模板
https://www.diycode.cc/topics/309
https://github.com/boredream/DesignResCollection



####  Robolectric

http://robolectric.org/getting-started/

[Robolectric](https://blog.csdn.net/qq_17766199/article/category/7226691)

```
testImplementation "org.robolectric:robolectric:3.8"

android {
  testOptions {
    unitTests {
      includeAndroidResources = true
    }
  }
}
```

