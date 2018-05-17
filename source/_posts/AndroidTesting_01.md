---
title: AndroidTesting_01
comments: true
date: 2018-01-22 21:58:05
tags: test
categories: ANDROID
---

### 　官方教程

https://www.jianshu.com/p/03118c11c199
https://github.com/googlesamples/android-testing

#### junit

Junit主要要做到Android.jar代码隔离，测试方式比较简单

```
public class CalculatorTest {

    private Calculator mCalculator;
    @Before
    public void setUp() throws Exception {
        mCalculator = new Calculator();
    }
    @Test
    public void testSum() throws Exception {
        //expected: 6, sum of 1 and 5
        assertEquals(6d, mCalculator.sum(1d, 5d), 0);
    }
    @Test
    public void testSubstract() throws Exception {
        assertEquals(1d, mCalculator.substract(5d, 4d), 0);
    }
    @Test
    public void testDivide() throws Exception {
        assertEquals(4d, mCalculator.divide(20d, 5d), 0);
    }
    @Test
    public void testMultiply() throws Exception {
        assertEquals(10d, mCalculator.multiply(2d, 5d), 0);
    }
}
```



Espresso测试

https://developer.android.com/training/testing/espresso/setup.html#add-espresso-dependencieshttps://developer.android.com/training/testing/espresso/basics.html










