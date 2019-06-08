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

使用方式：https://developer.android.com/training/testing/espresso/setup.html#add-espresso-dependencieshttps://developer.android.com/training/testing/espresso/basics.html

```
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".test.InstrumentActivity">

    <TextView
        android:id="@+id/tv_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <EditText
        android:id="@+id/et_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="@dimen/dimen_10"
        android:hint="输入姓名"
        app:layout_constraintTop_toBottomOf="@+id/tv_name" />

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="@dimen/dimen_10"
        android:onClick="sayClick"
        android:text="Say HELLO"
        app:layout_constraintTop_toBottomOf="@+id/et_name" />
</android.support.constraint.ConstraintLayout>
```

```
    public void sayClick(View v) {
        TextView textView = findViewById(R.id.tv_name);
        EditText editText = findViewById(R.id.et_name);
        textView.setText("Say HELLO!" + editText.getText().toString() + "!");
    }
```

```
@RunWith(AndroidJUnit4.class)
@LargeTest
public class InstrumentActivityTest {
    private static final String STRING_TO_BE_TYPED = "Jon";
    @Rule
    public ActivityTestRule<InstrumentActivity> instrumentActivityTestRule = new ActivityTestRule<>(InstrumentActivity.class);

    @Test
    public void sayHelloTest() {
        Espresso.onView(ViewMatchers.withId(R.id.et_name)).perform(ViewActions.typeText(STRING_TO_BE_TYPED), closeSoftKeyboard());
        Espresso.onView(ViewMatchers.withText("Say HELLO")).perform(click());


        String expectedText = "Say HELLO!" + STRING_TO_BE_TYPED + "!";
        Espresso.onView(ViewMatchers.withId(R.id.tv_name)).check(ViewAssertions.matches(ViewMatchers.withText(expectedText)));
    }
}
```





https://testing.googleblog.com/2008/12/static-methods-are-death-to-testability.html

<https://www.jianshu.com/p/dc30338a3e84>


