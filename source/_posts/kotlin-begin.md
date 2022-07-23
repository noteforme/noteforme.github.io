---
title: kotlin_begin
comments: true
date: 2019-11-02 16:08:22
tags:
categories: Kotlin
---

#### Kotlin in Action



##### 函数类型

```
val d: (String, Int) -> Unit // 函数类型 (String,Int) 接收参数 ，  Unit 返回类型
```





##### kotlin lambda



![2020-08-01-6.39.38](kotlin-begin/2020-08-01-6.39.38.png)



```
    val list = listOf("apple", "pear", "banana", "watermelon")

//    val result = list.maxBy { it.length }

    val lambda = { fruit: String ->
        fruit.length
    }

    //推导1
//    val result = list.maxBy(lambda)

    //推导2
//    val result = list.maxBy({fruit:String->
//        fruit.length
//    })


    //推导3
//    val result = list.maxBy({ fruit: String ->
//        fruit.length
//    })           //如果一个函数， 他接收的最后一个参数是lambda表达式，那么这最后一个参数可以移到括号外面

    //推导4
//    val result = list.maxBy{ fruit: String ->
//        fruit.length         //如果一个函数 ,括号可以省略
//    }

    //推导4
//    val result = list.maxBy{ fruit ->
//        fruit.length         //kotlin类型推导机制，参数类型可以省略
//    }

    //推导5
    val result = list.maxBy{
        it.length         // lmbda表达式只有一个参数，参数可以省略 改用it替代
    }
    
    println(result)
```



```
fun <T,R:Comparable<R>> List<T>.findMax(block: (T) -> R):T?{
    if (isEmpty()) return null
    var maxElement =get(0) //获取List上下文的 第一个元素
    var maxValue = block(maxElement) //block() 走到了 lambda方法中
    for(element in this){
        var value = block(element)
        if (value> maxValue){
            maxElement = element
            maxValue = value
        }
    }
    return maxElement

}
```

```
//    val result = list.findMax(lambda)
    val result = list.findMax{it.length}

    println(result)
```

https://www.bilibili.com/s/video/BV1Ut4y127xM



1.Java定义的接口

2.单抽象方法



##### Defining and calling functions

* *3.2.3 Getting rid of static utility classes: top-level functions and properties*

```
public class JoinJava {
    public static void johnToString(){
        System.out.println("johnToString java");
    }
}
```

```
package com.kotlin3


fun johnToString(){
    println("johnToString kotlin")
}
```



```
fun main(args: Array<String>) {
    johnToString()
    JoinJava.johnToString()
}
```



* *3.3.3 extension functions*

  ```
  fun String.lastChar() = this.get(this.length-1)
  
  fun String.addName() = this + " john"	//this 获取到String类的上下文
  
  
  val String.lastLetter: Char get()= get(length-1)  //拓展属性
  
  
  fun String.hellowrod(){
      println("hello world")
  }
  
  fun String.cpaitalEnd():String{
      if (this.isEmpty()) return "";
      val charArray = this.toCharArray()
      charArray[length-1] = charArray[length-1].toUpperCase()
      return String(charArray)
  }
  ```

  ```
  import com.kotlin3.t3_3_2.addName
  import com.kotlin3.t3_3_2.lastChar
  import com.kotlin3.t3_3_2.lastLetter
  
  fun main(args: Array<String>) {
     println("kotlin".lastChar())
      println("kotlin".addName())
  
      print("kotlin".lastLetter)
      
      "".hellowrod() //输出  hello world
      println("code".cpaitalEnd())	// codE
  
  }
  ```





##### *Conventions used*  7.3

>  如果用 10..20构建一个普通的 区间(闭区间)，该区间则包括10到20的所有数字， 包括20。开区间10 until 20 包括从 10 到凹的数字，但不包括 20。矩形类通常定义成这样，它的底部和右 侧坐标不是矩形的一部分，因此在这里使用开区 间是合适的 。



* Delegated - classs

  ```
  //原文链接：https://blog.csdn.net/xlh1191860939/article/details/99641573
  interface ServiceApi {
      fun login(userName: String, password: String)
  }
  
  class Retrofit : ServiceApi {
      override fun login(userName: String, password: String) {
          println("login successfully.")
      }
  }
  
  
  /**
   * 不用委托
   */
  //class RemoteRepository : ServiceApi, NewsApi {
  //
  //    private val serverApi: ServiceApi = Retrofit()
  //    private val retrofitApi: NewsApi = NewsApiImpl()
  //
  //    override fun login(username: String, password: String) {
  //        serverApi.login(username, password)
  //    }
  //
  //    override fun getNewsList() {
  //        retrofitApi.getNewsList()
  //    }
  //}
  
  
  
  
  /*
   *  使用kotlin 委托用下面简化
   */
  
  class RemoteRepository(): ServiceApi by Retrofit() , NewsApi by NewsApiImpl()
  
  
  
  interface NewsApi {
      fun getNewsList()
  }
  
  class NewsApiImpl : NewsApi {
      override fun getNewsList() {
          println("NewsApiImpl: getNewsList()")
      }
  }
  
  
  
  fun main(args: Array<String>) {
      val repository = RemoteRepository()
      repository.login("David", "123456") //输出 login successfully.
  
      repository.getNewsList()
  }
  ```

* 7.5.2 使用委托属性:惰性初始化和 “by lazy()”

```
//class Person(val name: String) {
//    private var _emails: List<Email>? = null
//    val emails: List<Email>
//        get() {
//            if (_emails == null) {
//                _emails = loadEmails(this)
//            }
//            return _emails!!
//        }
//}

//你有一个属性， emails，用来存储这个值， 而另一个 emails，用来提供对属性的读取访 问 。你需要使用两个属性 ，因为属性 具有不同的类型: emails 可以为空，而 emails 为非空。这种技术经常会使用到， 值得熟练掌握。

class Person(val name:String){
    val emails by lazy{
        loadEmails(this)
    }
}


fun loadEmails(person: Person): List<Email>? {
    println ("load emails for ${person.name }" )
    return listOf()
}

fun main(args: Array<String>) {
    val p = Person("Alice")
    p.emails
    p.emails
}
```

https://juejin.im/post/5e1288d86fb9a048217a19d9

https://zhuanlan.zhihu.com/p/65914552



##### 委托Shareprefernce实战

* 简易版本

  ```
  class DelegateSharedPreferencesUtils {
      object User : Delegates() {
          override fun getSharedPreferencesName(): String = this.javaClass.simpleName
          var name by string()
          var phone by long()
      }
  
  
      abstract class Delegates {
          private val preferences: SharedPreferences by lazy {
              AppUtil.getApp().applicationContext.getSharedPreferences(
                  getSharedPreferencesName(),
                  Context.MODE_PRIVATE
              )
          }
  
          abstract fun getSharedPreferencesName(): String
  
  
          fun int(defaultValue: Int = 0) = object : ReadWriteProperty<Any, Int> {
              override fun getValue(thisRef: Any, property: KProperty<*>): Int {
                  return preferences.getInt(property.name, defaultValue)
              }
  
              override fun setValue(thisRef: Any, property: KProperty<*>, value: Int) {
                  preferences.edit().putInt(property.name, value).apply()
              }
          }
  
          fun string(defaultValue: String? = null) = object : ReadWriteProperty<Any, String?> {
              override fun getValue(thisRef: Any, property: KProperty<*>): String? {
                  return preferences.getString(property.name, defaultValue)
              }
  
              override fun setValue(thisRef: Any, property: KProperty<*>, value: String?) {
                  preferences.edit().putString(property.name, value).apply()
              }
          }
  
          fun long(defaultValue: Long = 0L) = object : ReadWriteProperty<Any, Long> {
  
              override fun getValue(thisRef: Any, property: KProperty<*>): Long {
                  return preferences.getLong(property.name, defaultValue)
              }
  
              override fun setValue(thisRef: Any, property: KProperty<*>, value: Long) {
                  preferences.edit().putLong(property.name, value).apply()
              }
          }
  
          fun boolean(defaultValue: Boolean = false) = object : ReadWriteProperty<Any, Boolean> {
              override fun getValue(thisRef: Any, property: KProperty<*>): Boolean {
                  return preferences.getBoolean(property.name, defaultValue)
              }
  
              override fun setValue(thisRef: Any, property: KProperty<*>, value: Boolean) {
                  preferences.edit().putBoolean(property.name, value).apply()
              }
          }
  
          fun float(defaultValue: Float = 0.0f) = object : ReadWriteProperty<Any, Float> {
              override fun getValue(thisRef: Any, property: KProperty<*>): Float {
                  return preferences.getFloat(property.name, defaultValue)
              }
  
              override fun setValue(thisRef: Any, property: KProperty<*>, value: Float) {
                  preferences.edit().putFloat(property.name, value).apply()
              }
          }
  
          fun setString(defaultValue: Set<String>? = null) = object :
              ReadWriteProperty<DelegateSharedPreferencesUtils, Set<String>?> {
              override fun getValue(
                  thisRef: DelegateSharedPreferencesUtils,
                  property: KProperty<*>
              ): Set<String>? {
                  return preferences.getStringSet(property.name, defaultValue)
              }
  
              override fun setValue(
                  thisRef: DelegateSharedPreferencesUtils,
                  property: KProperty<*>,
                  value: Set<String>?
              ) {
                  preferences.edit().putStringSet(property.name, value).apply()
              }
          }
  
          fun clearAll() {
              preferences.edit().clear().apply()
          }
      }
  }
  ```

  

  

  ```
  DelegateSharedPreferencesUtils.User.name = "john" //存
  
  bt_get.setOnClickListener {
      Timber.i("bt_get    "+DelegateSharedPreferencesUtils.User.name) //取
  }
  ```

  https://wazing.github.io/2019/05/23/kotlin-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%A7%94%E6%89%98%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0SharedPreferences/

* Pro version

  ```
  object SpUtil {
      val SP by lazy {
          AppUtil.getApp().getSharedPreferences("default", Context.MODE_PRIVATE)
      }
  
      //读 SP 存储项
      fun <T> getValue(name: String, default: T): T = with(SP) {
          val res: Any = when (default) {
              is Long -> getLong(name, default)
              is String -> getString(name, default) ?: ""
              is Int -> getInt(name, default)
              is Boolean -> getBoolean(name, default)
              is Float -> getFloat(name, default)
              else -> throw java.lang.IllegalArgumentException()
          }
          res as T
      }
  
      //写 SP 存储项
      fun <T> putValue(name: String, value: T) = with(SP.edit()) {
          when (value) {
              is Long -> putLong(name, value)
              is String -> putString(name, value)
              is Int -> putInt(name, value)
              is Boolean -> putBoolean(name, value)
              is Float -> putFloat(name, value)
              else -> throw IllegalArgumentException("This type can't be saved into Preferences")
          }.apply()
      }
  
  }
  ```



```
/**
 * @author xiaofei_dev
 * @desc 定义的 SP 存储项
 */
object SpBase{
    //SP 存储项的键
    private const val CONTENT_SOMETHING = "CONTENT_SOMETHING"


    // 这就定义了一个 SP 存储项
    // 把 SP 的读写操作委托给 SPDelegates 类的一个实例（使用 by 关键字，by 是 Kotlin 语言层面的一个原语），
    // 此时访问 SpBase 的 contentSomething (你可以简单把其看成 Java 里的一个静态变量)属性即是在读取 SP 的存储项，
    // 给 contentSomething 属性赋值即是写 SP 的操作，就这么简单
    // 这里用到的 SPDelegates 对象的 getValue 方法的 thisRef（见上文） 参数的类型正是外层的 SpBase
    var contentSomething: String by SPDelegates(CONTENT_SOMETHING, "我是一个 SP 存储项，点击编辑我")
}



class SPDelegates<T>(private val key: String, private val default: T) : ReadWriteProperty<Any?, T> {
    override fun getValue(thisRef: Any?, property: KProperty<*>): T {
        return SpUtil.getValue(key, default)
    }
    override fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
        SpUtil.putValue(key, value)
    }
}
```



```
    SpBase.contentSomething = "显示"
       

    bt_get.setOnClickListener {
          Timber.i("bt_get    "+SpBase.contentSomething)
    }
```



https://juejin.im/post/5e1bbf396fb9a02ffe702610

##### Operator

* FindViewById

  ​	Kotlin不用findViewById 注意在Fragmet中 需要在onViewCreated后使用

  ​	 https://blog.csdn.net/hust_twj/article/details/80290362 

  * The reason why we ignore findviewbyId

    https://antonioleiva.com/kotlin-android-extensions/ 

?  

```
//kotlin:
a?.foo()

//相当于java:
if(a!=null){
  a.foo();
}else{
   null 
}
```

!!

```
//kotlin:
a!!.foo()

//相当于java:  
if(a!=null){
  a.foo();
}else{
  throw new KotlinNullPointException();
}
```

* null or empty

```
    data = " " // this is a text with blank space 

    println(data.isNullOrBlank()?.toString())  //true
    println(data.isNullOrEmpty()?.toString())  //false
```

* Elvis

```
val ss:String?=null
println(ss ?: "1")

>> 1
```







https://developer.android.com/samples/?language=kotlin

https://developer.android.com/kotlin/get-started

https://www.jianshu.com/p/9fb9a1ab6c31

https://juejin.im/post/5aa64556f265da238c3a51d3



#### 高级用法

<https://www.cnblogs.com/Jetictors/p/9225557.html>



##### by

https://blog.csdn.net/wzgiceman/article/details/82689135

oprator

https://zhuanlan.zhihu.com/p/26546977



####  kotlin细节

https://juejin.im/post/5eeffd73f265da02ec0bc42e

1. apply 

   ```
   val mDialPaint = Paint(Paint.ANTI_ALIAS_FLAG)
   mDialPaint.apply {
               color = Color.parseColor("#3333333")
               strokeWidth = arcWidth*8
               textSize = 65f
           }
   ```



#####  coroutines

https://www.kotlincn.net/docs/reference/coroutines/coroutines-guide.html

https://juejin.im/post/5eea757a51882565ae5cb287

https://blog.csdn.net/xlh1191860939/article/details/99641573



##### Android kotlin计划

ktx不是重心，重心在kotlin库如协程,paging3.0和compose

https://www.bilibili.com/video/BV1fV411r7Lw



##### string to list. list. to string

https://www.bezkoder.com/kotlin-convert-string-list/



### const

val 加不加const的区别

Const 'val' are only allowed on top level, in named objects, or in companion objects

const只能在下面3种情况加

```kotlin
val name = "jon"
const val age = "joo"

object ConstantObject {
    val name = "jon"
    const val age = "joo"
}
class ConstantClass {
    companion object {
        val name = "jon"
        const val age = "joo"
    }
}
```



上面三种 反编译后，都是下面这种形式

```java
@NotNull
private static final String name = "jon";
@NotNull
public static final String age = "joo";

@NotNull
public static final String getName() {
   return name;
}
```



不加const , private修饰，通过get获取， 会有一点点效率影响

https://www.cnblogs.com/liuliqianxiao/p/7253116.html
