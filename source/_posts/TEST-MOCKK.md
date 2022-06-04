---
title: TEST-MOCKK
comments: true
date: 2018-01-22 18:04:40
tags: Test
categories: TEST
---





https://mockk.io/



presenter 

https://www.jianshu.com/p/899e80120071

https://blog.csdn.net/hard_working1/article/details/105613362

https://juejin.cn/post/6877824384694747143



#### LIST COMPARE

```kotlin
 		@Test
    fun `compare to list`() {

        val actual = listOf("a", "b", "c")
        val expected = listOf("a", "b", "c")

        //All passed / true

        //1. Test equal.

        //All passed / true

        //1. Test equal.
        Assert.assertThat(actual, CoreMatchers.`is`(expected))

        //2. If List has this value?

        //2. If List has this value?
        Assert.assertThat(actual, CoreMatchers.hasItems("b"))

        //3. Check List Size

        //3. Check List Size
        Assert.assertThat(actual, Matchers.hasSize(3))

        Assert.assertThat(actual.size, CoreMatchers.`is`(3))

        //4.  List order

        // Ensure Correct order

        //4.  List order

        // Ensure Correct order
        Assert.assertThat(actual, Matchers.contains("a", "b", "c"))

        // Can be any order

        // Can be any order
        Assert.assertThat(actual, IsIterableContainingInAnyOrder.containsInAnyOrder("c", "b", "a"))

        //5. check empty list

        //5. check empty list
        Assert.assertThat(actual, CoreMatchers.not(IsEmptyCollection.empty()))

//        assertThat(ArrayList(), IsEmptyCollection.empty())

    }
```



https://stackoverflow.com/questions/46788032/compare-2-liststring-if-they-contain-same-elements-in-any-order-junit-asset
https://mkyong.com/unittest/junit-how-to-test-a-list/

http://hamcrest.org/JavaHamcrest/distributables



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

https://www.jianshu.com/p/0a8bbfe6cba2



实例讲解

 http://blog.csdn.net/qq_17766199/article/details/78450007
 https://github.com/ChrisZou/android-unit-testing-tutorial
https://github.com/qingmei2/Sample_AndroidTest

一个Android项目搞定所有主流架 mvp生成模板
https://www.diycode.cc/topics/309
https://github.com/boredream/DesignResCollection





###  Robolectric

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



### Mockk



##### Constructor mocks

verify失败



```kotlin
  @Test
    fun `mock different constuctor`(){
        mockkConstructor(MockCls::class)

        every { constructedWith<MockCls>().add(1) } returns 2
        every {
            constructedWith<MockCls>(OfTypeMatcher<String>(String::class)).add(2) // Mocks the constructor which takes a String
        } returns 3
        every {
            constructedWith<MockCls>(EqMatcher(4)).add(any()) // Mocks the constructor which takes an Int
        } returns 4

        assertEquals(1, MockCls().add(1))
        assertEquals(3, MockCls("2").add(2))
        assertEquals(4, MockCls(4).add(7))

        verify {
            constructedWith<MockCls>().add(1) //这里不通过 
//            constructedWith<MockCls>("2").add(2)
            constructedWith<MockCls>(EqMatcher(4)).add(7)
        }
```



##### 为无返回值的方法分配默认行为

把 every {…} 后面的 Returns 换成 just Runs ，就可以让 MockK 为这个没有返回值的方法分配一个默认行为。

```kotlin
@Test
fun testGetGoods() {
    val goods = presenter!!.getGoods(1)
    every { view.showLoading() } just Runs
    verify { view.showLoading() }
    assertEquals(goods.name, "纸巾")
}
```

##### 为所有模拟对象的方法分配默认行为

```kotlin
@Before
fun setUp() {
    MockKAnnotations.init(this, relaxed = true)
}
```



##### Annotation

只要在 `@Before` 方法裡面加上 `MockKAnnotations.init(this)` 就能在外面用 Annotation 的方式 Mock 物件。

```kotlin
class KidAnnotationTest {

    @MockK
    lateinit var mother: Mother

    lateinit var kid: Kid

    @Before
    fun setUp() {
   			val mother = mockk<Mother>() // 不使用Annotation的版本
        
        MockKAnnotations.init(this)
        kid = Kid(mother)
    }

    @Test
    fun wantMoney() {
        every { mother.giveMoney() } returns 30
        kid.wantMoney()
        assertEquals(30, kid.money)
    }
}
```



https://proandroiddev.com/understanding-unit-tests-for-android-in-2021-71984f370240

https://medium.com/joe-tsai/mockk-%E4%B8%80%E6%AC%BE%E5%BC%B7%E5%A4%A7%E7%9A%84-kotlin-mocking-library-part-2-4-4be059331110

https://tngdigital.yuque.com/tngd-mobile/key-battle/wldcgt
