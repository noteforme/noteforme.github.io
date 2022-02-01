---
title: kotlin-senior2
comments: true
date: 2021-09-11 18:04:06
tags:
categories: Kotlin
---



### Lambda表达式

**Lambda表达式、匿名函数、双冒号+函数名这三个东西，都是函数类型的对象，他们都能够赋值给变量以及当作函数的参数传递！**

##### Lambda表达式的格式

* Lambda表达式被大括号包围着

* Lambda表达式的参数在`->`的左边，如果没有参数，则只保留函数体

* Lambda表达式的函数体在`->`的后面

* Lambda表达式的返回类型值总为函数体最后一行代码的返回值类型

##### 无参数，无返回值的Lambda表达式

```kotlin
val test01Lambda = {
    print("无参数,无返回值")
}
```

我们同样将Lambda表达式赋值给了变量，这个变量是函数类型（`() -> Unit`）的对象。

##### 有参数，无返回值的Lambda表达式



```kotlin
val test02Lambda = { name: String ->
    print("有参数,无返回值，参数值为：$name")
}
```

这个变量是函数类型（`(String) -> Unit`）的对象。

如果我们手动给这个变量指明了类型，那么Lambda的参数类型还可以不写：

注意这个name参数 是回调方法传过来的参数.

```kotlin
val test02Lambda: (String) -> Unit = { name ->
    print("有参数,无返回值，参数值为：$name")
}
```

这个时候，Kotlin可以自动为我们推断出Lambda中这个参数的类型是String类型。

如果Lambda表达式的参数只有一个，我们甚至连这个参数都可以省略不写，那...我想使用这个参数的时候怎么办呢？我们可以用`it`来代替：

```kotlin
val test02Lambda: (String) -> Unit = {
    print("有参数,无返回值，参数值为：$it")
}
```

##### 有参数，有返回值的Lambda表达式

```kotlin
val test03Lambda = { doubleValue: Double ->
    print("parameter is Double,Value is:$doubleValue")
    print("now parse Double into String")
    doubleValue.toString()
}
```

Lambda表达式的最后一行代码将作为返回值，因此它对应的函数类型为：`(Double) -> String`。同样，如果变量已经确切的指定了类型，则Lambda表达式的参数类型可以省略，又由于只有一个参数，所以连参数都可以省略，Kotlin将使用it代替这个参数名。



```kotlin
val method: (String, String) -> Unit = { aStr, bStr -> println("a : $aStr, b:$bStr") }
method("john","男")

val method02 = { println("john")}
val method03:(String)->Unit={
    println("传入的是:$it")
}
method03("Jon")
val method04:(Int)->Unit={
    when(it){
        1-> println("等于1")
        in 20.. 30 -> println("20 - 30的数字")
        else -> println("都不满足")
    }
}
method04(1)
val method05 : (Int,Int)-> Unit = {aNumber,bNumber->
    println("第一个: $aNumber, 第二个 : $bNumber")
}
method05(1,9)

val method06 : (Int,Int)-> Unit = {aNumber,_->
    println("第一个: $aNumber")
}
method06(99,88)
val method07 : (String)->String = {str -> str}
println(method07("Jon"))
```







### 函数类型

##### 函数类型的书写格式

所有函数类型都有一个圆括号括起来的参数类型列表，以及一个返回类型：`(A, B) -> C` ，参数列表与返回值类型之间通过 -> 符号连接。`(A, B) -> C`表示接收类型分别为 `A` 与 `B` 两个参数并返回一个 `C` 类型值的函数类型。

```kotlin
val f: (Int) -> String
```

`(Int) -> String`这就是一个函数类型。另外需要注意的是，即使函数类型的参数列表为空，也必须保留小括号，例如`() -> String`；即使函数类型的返回值类型为Unit，也不可省略Unit不写，例如：`(Int) -> Unit`。



##### 函数类型的对象使用



`sum(1, 2)`这我可以理解，毕竟sum是一个函数，但是`block(1, 2)`这样也行？block可是一个函数类型的对象啊，函数类型的对象后面可以加括号调用？

没错，只有函数类型的对象可以，其实这是Kotlin为我们提供的一个语法糖，它本质上还是会去调用invoke函数，也就是说

```kotlin
val block: (Int, Int) -> Int = ::sum

fun sum(firstNumber: Int, secondNumber: Int): Int {
    return firstNumber + secondNumber
}
```

```kotlin
val block: (Int, Int) -> Int = ::sum

// 以下调用都是有返回值的,因为(Int, Int) -> Int这个函数类型是有返回值的
block(1, 2)
// 等价于
block.invoke(1, 2)
// 等价于
(::sum)(1, 2)
// 等价于
(::sum).invoke(1, 2)
```

Kotlin使用这种（函数类型对象能够使用括号访问）语法让我们感觉，嗯，block就是sum函数的替身，而实际上它是一个对象，一个函数类型的对象。

### 高阶函数

##### 什么是高阶函数

```java
btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
    }
});
```

有没有想过为什么需要传一个匿名类过去？因为Framework会在该View被点击的时候调用我们在匿名类中实现的那个onClick方法，以便于执行我们自己的处理逻辑。

要是我们能直接将方法传过去就好了...梦该醒了，这是Java，只能通过接口 + 匿名内部类这种折中的方案来实现这样的需求。

而Kotlin有了函数类型，又有了高阶函数，意味着我们可以传入一段代码给函数了，函数可以在适当的时候执行我们传入的那段代码，完美的解决了这一痛点。





##### 高阶函数的使用案例

虽然都是过滤找出符合要求的苹果，但是条件是不一样的。这个怎么做呢？最简单的方法就是写三个循环，每个循环中针对不同的条件对苹果集合进行筛选。但是我们完全可以使用高阶函数和函数类型优雅的实现这个功能：

```kotlin
data class AppleBean(val color:Int, val weight:Int)
```



```kotlin
private fun filterApple(appleList: List<AppleBean>, predicate: (AppleBean) -> Boolean): List<AppleBean> {
    val destination = mutableListOf<AppleBean>()
    for (appleBean in appleList) {
        if (predicate(appleBean)){
            destination.add(appleBean)
        }
    }
    return destination
}
```

，filterApple函数主要的功能只是遍历集合，以及把符合条件的苹果添加到新的集合中返回，至于筛选的条件，filterApple函数并不知道，而是完全取决于predicate这个函数类型的参数，只要predicate返回true，则符合条件。这样我们可以在调用函数时动态地传入不同的过滤规则

```kotlin
fun main() {
    filterApple(appleList, ::filterColorPredicate)
    filterApple(appleList, ::filterWeightPredicate)
    filterApple(appleList, ::filterColorAndWeightPredicate)
}

private fun filterColorPredicate(appleBean: AppleBean): Boolean = appleBean.color == 0xFF0000

private fun filterWeightPredicate(appleBean: AppleBean): Boolean = appleBean.weight > 6

private fun filterColorAndWeightPredicate(appleBean: AppleBean): Boolean = appleBean.color == 0xFF0000 && appleBean.weight > 6

private fun filterApple(appleList: List<AppleBean>, predicate: (AppleBean) -> Boolean): List<AppleBean> {
    val destination = mutableListOf<AppleBean>()
    for (appleBean in appleList) {
        if (predicate(appleBean)) {
            destination.add(appleBean)
        }
    }
    return destination
}
```

这...太麻烦了吧，我每次调用高阶函数，传入函数类型的时候，还必须要先定义一个函数，然后再使用`::函数名`传入吗？其实不必这么麻烦的，现在我们学习的只不过是其中的一种方式而已。下面我们就要来学习更加简单的方式：匿名函数。

### Lambda表达式与高阶函数

现在大家已经对Lambda表达式的写法了如指掌了，现在是时候来看看Lambda表达式如何与高阶函数配合使用了。还记得之前的那个筛选苹果的例子吗？我们定义了一个高阶函数，它的名字叫：filterApple，代码如下：



```kotlin
private fun filterApple(appleList: List<AppleBean>, predicate: (AppleBean) -> Boolean): List<AppleBean> {
    val destination = mutableListOf<AppleBean>()
    for (appleBean in appleList) {
        if (predicate(appleBean)) {
            destination.add(appleBean)
        }
    }
    return destination
}
```

这个高阶函数的第二个参数接收了一个函数类型的参数，在之前，我们分别使用了两种方式进行调用：

1. 双冒号的方式：

   ```kotlin
   fun main() {
       filterApple(appleList, ::filterColorPredicate)
   }
   
   private fun filterColorPredicate(appleBean: AppleBean): Boolean = appleBean.color == 0xFF0000
   ```

2. 匿名函数的方式：

   ```kotlin
   filterApple(appleList, fun(appleBean: AppleBean): Boolean = appleBean.weight > 6)
   ```

3. 完整的写法

   ```kotlin
   filterApple(appleList, { appleBean: AppleBean -> appleBean.weight > 6 })
   ```

4. 简化的写法

   我们怎么知道哪个参数需要传入Lambda表达式呢？当然是看被调用的这个高阶函数的定义啊，还记的这个高阶函数的样子吗？

   我们怎么知道哪个参数需要传入Lambda表达式呢？当然是看被调用的这个高阶函数的定义啊，还记的这个高阶函数的样子吗？

   ```kotlin
   private fun filterApple(appleList: List<AppleBean>, predicate: (AppleBean) -> Boolean): List<AppleBean> {
       // ...
   }
   ```

   显然第二个形参是一个函数类型的参数，并且已经明确的指出了形参的函数类型：`(AppleBean) -> Boolean`（实际上也必须明确指出，否则报错）。根据我们前面所学习的知识，这种情况下，我们可以直接省略Lambda表达式中的参数类型，Kotlin会根据上下文自动推断：

   ```kotlin
   filterApple(appleList, { appleBean -> appleBean.weight > 6 })
   ```

5. 再简化的写法

   由于这个函数类型只需要一个参数，因此我们还可以省略参数的名字，Kotlin会使用it代替，这在之前的Lambda讲解中，都是讲过的：

   ```kotlin
   filterApple(appleList, { it.weight > 6 })
   ```

6. 再再简化的写法

   第一个是：如果Lambda表达式作为函数的最后一个参数传入，那么它可以单独放在调用函数的括号后面：

   ```kotlin
   filterApple(appleList) { it.weight > 6 }
   ```

   第二个是：如果函数只接收一个函数类型的参数，我们传入Lambda表达式时，连函数调用的括号都可以去掉：

   ```kotlin
   fun main() {
       test { num1, num2 ->
           // ...
       }
   }
   
   private fun test(block: (Int, Int) -> Unit) {
       // ...
   }
   ```



​		作者：HurryYu_YZH
​		链接：https://www.jianshu.com/p/8eb0623f08c6







### 项目实例

##### Login01.kt

```kotlin
//传入一个具名函数，被调用后，获取参数
fun test(resModel: ResModel){
    println(resModel.int)
}

class LoginModelImpl {
    fun loginOptions(
        mobileNo: String,
        metaInfo: String,
        block: (model: ResModel) -> Unit
    ) {
        val model = ResModel(8)
        block(model)
      //  block.invoke(model) 这样写也可以 
    }
}

class ResModel(val int: Int)
```



```kotlin
    val loginModelImpl = LoginModelImpl()
    val responseModel = ResModel(0)
    loginModelImpl.loginOptions("","",::test) // 可以看到 fun test(resModel: ResModel)也被调用,可以理解被回调
//    loginModelImpl.loginOptions("","",{model: ResModel -> model.toString() })
//    loginModelImpl.loginOptions("","",{model -> model.toString() })
    loginModelImpl.loginOptions("","",{println(it.int)}) //lambda只有一个参数，传入参数也可以省略,{}里面用it代替
//    loginModelImpl.loginOptions("","",{}) // it可以用也可以不用
```



##### Login02.kt

```kotlin
fun main() {
    loginAction("Jo1n", "123456") {
        if (it) {
            println("登录成功")
        } else
            println("登录失败")
    }
}

fun loginAction(userName: String, pwd: String, loginResponseResult: (Boolean) -> Unit) {
    if (userName == null || pwd == null) return
    loginEngine(userName, pwd, loginResponseResult)
}

fun loginEngine(userName: String, pwd: String, loginResponseResult: (Boolean) -> Unit) {
    if ("Jon" == userName && "123456" == pwd) {
        loginResponseResult(true)
    } else
        loginResponseResult(false)
}
```

 https://www.bilibili.com/video/BV1xv411k7Dd?p=3&spm_id_from=pageDriver



##### updateFastClickTime

这个方法很形象, 匿名函数的参数 ,是回调时候传的

```kotlin
fun updateFastClickTime(callback: ((currentTime: Long, lastClickTime: Long) -> Unit)? = null): Long {
    val curClickTime = System.currentTimeMillis()
    callback?.invoke(
        curClickTime,
        lastClickTime
    )
    lastClickTime = curClickTime
    return curClickTime
}
```



##### getOrElse

 匿名函数的参数 ,是回调时候传的,返回T类型.

```
inline fun <T> List<T>.getOrElse(

    index: Int,

    defaultValue:(Int) -> T       //这个 (Int) -> T 看不明白

): T


public inline fun <T> Array<out T>.getOrElse(index: Int, defaultValue: (Int) -> T): T {
    return if (index >= 0 && index <= lastIndex) get(index) else defaultValue(index)
}

```

https://bbs.csdn.net/topics/399173319
