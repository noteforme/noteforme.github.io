---
title: INTERVIEW-JAVA
comments: true
date: 2018-05-24 22:31:57
tags: 
categories: INTERVIEW

---



[面试题含答案](https://www.cnblogs.com/huangjialin/p/12411842.html)

###  （一） java基础面试知识点

#### java中==和equals和hashCode的区别

1. 比较对象,

   当两个引用地址相同， == 返回true,equals()返回取决于重写的实现.

2. 基础数据类型比较

   比较的值是否相等.

3. ?  JVM String特性 

   Java虚拟机在内存中开辟出一块单独的区域

   https://zhuanlan.zhihu.com/p/60643031
   
   https://www.bilibili.com/video/BV1PJ411n7xZ?p=118   几个视频讲到用法，最好能用图画出来
   
   https://www.iteye.com/blog/rednaxelafx-774673

​		

#### int、char、long各占多少字节数

   byte   1字节       short   2字节         int    4字节           long   8字节          

   char   2字节       float   4字节         double  8字节      boolean  false/true(理论上占用1bit,1/8字节，实际处理按1byte处理) 

#### int与integer的区别

​	Integer 是int的包装类；int是基本数据类型;

​	Integer实际是对象的引用，int是直接存储数据值

#### 谈谈对java多态(polymorphism)的理解,Java中实现多态的机制是什么

 实现的机制是，父类或者接口定义的引用变量指向子类或者子类的实现， 执行期间判断所引用对象的实际类型，根据其实际的类型调用相应的方法。

1. 编译时多态(静态多态)

2. 运行时多态（动态多态）

   

   无论哪种方法，核心之处在对父类方法的改写或对接口方法的实现，以取得运行时不同的执行效果.

https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html

https://cloud.tencent.com/developer/article/1447574



#### String、StringBuffer、StringBuilder区别

 * 都是fianl类，不能被继承,底层都是 char[] value实现
 * String类长度是不可变的，substring()、  concat(),最终实现都是通过  new String(buf, true)实现的,StringBuffer,StringBuilder是通过操作本类的value实现的
 * StringBuffer类是线程安全的，StringBuilder不是线程安全的

​	

#### 什么是内部类？内部类的作用

​	内部类:	一个类定义在另一个类的内部，就叫内部类

​	作用: 

 * 内部类  拥有外部类的所有访问权限，包括被private修饰的私有数据

 * 内部类可以很好的隐藏实现

 * 内部类可以实现多重继承

    ```java
    //类一
    public class ClassA {
       public String name(){
           return "liutao";
       }
       public String doSomeThing(){
        // doSomeThing
       }
    }
    //类二
    public class ClassB {
        public int age(){
            return 25;
        }
    }
    
    //类三
    public class MainExample{
       private class Test1 extends ClassA{
            public String name(){
              return super.name();
            }
        }
        private class Test2 extends ClassB{
           public int age(){
             return super.age();
           }
        }
       public String name(){
        return new Test1().name();
       }
       public int age(){
           return new Test2().age();
       }
       public static void main(String args[]){
           MainExample mi=new MainExample();
           System.out.println("姓名:"+mi.name());
           System.out.println("年龄:"+mi.age());
       }
    }
    
    ```

    MainExample 类通过内部类拥有了 ClassA 和 ClassB 的两个类的继承关系。 而无需关注 ClassA 中的 doSomeThing 方法的实现。这就是比接口实现更有戏的地方

    https://juejin.cn/post/6844903566293860366

####  抽象类和接口区别

| Abstract class                                               | Interface                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1) Abstract class can **have abstract and non-abstract** methods. | Interface can have **only abstract** methods. Since Java 8, it can have **default and static methods** also. |
| 2) Abstract class **doesn't support multiple inheritance**.  | Interface **supports multiple inheritance**.                 |
| 3) Abstract class **can have final, non-final, static and non-static variables**. | Interface has **only static and final variables**.           |
| 4) Abstract class **can provide the implementation of interface**. | Interface **can't provide the implementation of abstract class**. |
| 5) The **abstract keyword** is used to declare abstract class. | The **interface keyword** is used to declare interface.      |
| 6) An **abstract class** can extend another Java class and implement multiple Java interfaces. | An **interface** can extend another Java interface only.     |
| 7) An **abstract class** can be extended using keyword "extends". | An **interface** can be implemented using keyword "implements". |
| 8) A Java **abstract class** can have class members like private, protected, etc. | Members of a Java interface are public by default.           |
| 9)**Example:**  public abstract class Shape{ public abstract void draw(); } | **Example:**  public interface Drawable{ void draw(); }      |

https://www.javatpoint.com/difference-between-abstract-class-and-interface

 

#### ? 泛型中extends和super的区别

- 上界<? extends T>不能往里存，只能往外取，适合频繁往外面读取内容的场景。

- 下界<? super T>不影响往里存，但往外取只能放在Object对象里，适合经常往里面插入数据的场景

  https://noteforme.github.io/2018/04/16/Generics/

#### string 转换成 integer的方式及原理

  1. 判断是否null或""
  2. 判断第一位正负数，逐位获取值 ?

  https://blog.csdn.net/nobody_1/article/details/91488686

####   静态内部类的设计意图

​	非静态内部类编译后会隐含的保存着一个引用，改引用指向创建它的外围类，静态内部类没有，

​	它不能使用任何外围类的非static成员变量和方法.

#### final，finally，finalize的区别

* Final : 用于申明属性，方法，类，表示属性不可变，方法不可以覆盖，类不能继承

1. final数组

   **Java 中数组也是对象**

   ```java
     final int arr[] = {1, 2, 3, 4, 5};  //  注意，数组 arr 是 final 的
          for (int i = 0; i < arr.length; i++) {
              arr[i] = arr[i]*10;
              System.out.println(arr[i]);
          }
   ```

   数组是对象的一种，现在数组是被 final 修饰的，所以它的意思是一旦被赋值之后，变量的引用不能修改。但是我们现在想证明的是，数组对象里面的内容可以修改

2. 非数组对象

   ```java
   class Test { 
       int p = 20; 
       public static void main(String args[]){ 
          final Test t = new Test();
          t.p = 30; 
          System.out.println(t.p);
       }
   }
   ```

   把它用 final 修饰，然后去尝试改它里面成员变量 p 的值，并打印出结果，程序会打印出“30”。一开始 p 的值是 20，但是最后修改完毕变成了 30，说明这次修改是成功的。

   以上我们就得出了一个结论，**final 修饰一个指向对象的变量的时候，对象本身的内容依然是可以变化的**。

   https://kaiwu.lagou.com/course/courseInfo.htm?courseId=16#/detail/pc?id=311



* Finally: 异常语句处理机构中，与try{}进行配合使用，不论try中的代码是否执行完，表示总是执行的部分

* Finalize: Object类的一个方法，用于对象"消失"时，由JVM进行调用用于对对象进行垃圾回收，释放对象占用				的资源.





#### Serializable 和Parcelable 的区别，如何将一个Java对象序列化到文件里？

1. `Parcelable` is faster than `Serializable` interface,ObjectOutputStream写入到文件中

2. Serializable: interface creates a lot of temporary objects and causes quite a bit of garbage collection

3. > Parcel is **not** a general-purpose serialization mechanism.  This class (and the corresponding `Parcelable` API for placing arbitrary objects into a Parcel) is designed as a high-performance IPC transport.  As such, **it is not appropriate to place any Parcel data in to persistent storage**: changes in the underlying implementation of any of the data in the Parcel can render older data unreadable. 很难持久化对象


   https://developer.android.com/reference/android/os/Parcel

   

父类的静态方法能否被子类重写

静态方法只和类有关,JVM加载后先初始化static相关属性方法,重写依赖于类的实例.

> **Overriding:** Overriding in Java simply means that the particular method would be called based on the run time type of the object and  not on the compile time type of it (which is the case with overriden  static methods)
>
> Overriding depends on having an instance of a class. The point of  polymorphism is that you can subclass a class and the objects  implementing those subclasses will have different behaviors for the same methods defined in the superclass (and overridden in the subclasses). A static method is not associated with any instance of a class so the  concept is not applicable.

   



#### 成员内部类、局部内部类以及项目中的应用

成员内部类 : 普通的内部类，不能存在任何static的变量和方法；

局部内部类： 嵌套于方法和作用域内

```java
public class Parcel5 {
    public Destionation destionation(String str){
        class PDestionation implements Destionation{
            private String label;
            private PDestionation(String whereTo){
                label = whereTo;
            }
            public String readLabel(){
                return label;
            }
        }
        return new PDestionation(str);
    }
    
  public static void main(String[] args) {
        Parcel5 parcel5 = new Parcel5();
      Destionation d = parcel5.destionation("chenssy");
    }
}
```



#### 谈谈对kotlin的理解

null处理

拓展函数

Findviewbyid

Class Map list 处理更加方便

Android 支持java8以后新特性支持, 配置比较麻烦。

闭包和局部内部类的区别

不知道有什么作用



### （二） java深入源码级的面试题（有难度）

#### 哪些情况下的对象会被垃圾回收机制处理掉？

1. 可达性分析,GC Root向下搜索，产生一个reference chain 。当一个对象不能和任何GC Root产生关系时就被回收.
2. * 强引用 
   * 软引用: 内存溢出时候对象回收
   * 弱引用:下一次GC时候对象回收
   * 虚引用：随时可能被回收

https://noteforme.github.io/2021/01/05/JVM-GC/

#### 讲一下常见编码方式？

1. ASCII码:  用一个字节的低7位表示，总共128个,0~31 是控制字符如换行回车删除等；32~126 是打印字符，可以通过键盘输入并且能够显示出来

2. UTF-16:  固定两个字节表示一个字符：说到 UTF 必须要提到 Unicode（Universal Code 统一码），ISO  试图想创建一个全新的超语言字典，世界上所有的语言都可以通过这本字典来相互翻译。可想而知这个字典是多么的复杂，关于 Unicode  的详细规范可以参考相应文档。Unicode 是 Java 和 XML 的基础，下面详细介绍 Unicode 在计算机中的存储形式。 
   UTF-16 具体定义了 Unicode 字符在计算机中存取方法。UTF-16 用两个字节来表示 Unicode  转化格式，这个是定长的表示方法，不论什么字符都可以用两个字节表示，两个字节是 16 个 bit，所以叫 UTF-16。UTF-16  表示字符非常方便，每两个字节表示一个字符，这个在字符串操作时就大大简化了操作，这也是 Java 以 UTF-16  作为内存的字符存储格式的一个很重要的原因。

3. UTF-8:  优化UTF-16,UTF-16  统一采用两个字节表示一个字符，虽然在表示上非常简单方便，但是也有其缺点，有很大一部分字符用一个字节就可以表示的现在要两个字节表示，存储空间放大了一倍，在现在的网络带宽还非常有限的今天，这样会增大网络传输的流量，而且也没必要。而 UTF-8 采用了一种变长技术，每个编码区域有不同的字码长度。不同类型的字符可以是由 1~6 个字节组成。 
   UTF-8 有以下编码规则： 
   如果一个字节，最高位（第 8 位）为 0，表示这是一个 ASCII 字符（00 - 7F）。可见，所有 ASCII 编码已经是 UTF-8 了。 
   如果一个字节，以 11 开头，连续的 1 的个数暗示这个字符的字节数，例如：110xxxxx 代表它是双字节 UTF-8 字符的首字节。 
   如果一个字节，以 10 开始，表示它不是首字节，需要向前查找才能得到当前字符的首字节 

4. GBK 、ISO-8859-1、GB2312

   https://www.cnblogs.com/mlan/p/7823375.html

5. https://www.cnblogs.com/mlan/p/7823375.html

#### utf-8编码中的中文占几个字节；int型几个字节？

   32位 64位代表寻址方式

​	少数是汉字每个占用3个字节，多数占用4个字节。

​	int类型 4个字节

#### 静态代理和动态代理的区别，什么场景使用？

​	静态代理 :  编译的时候就已经存在，

​	动态代理 ： 通过反射机制生成的代理对象

​	https://noteforme.github.io/2021/01/14/DesignPattern-Proxy/

#### Java的异常体系

非运行时异常 :编译期间可以检查到的异常, 像IoException,DataFormatException,CertificateException

运行时异常 : NullPointerException, ClassCastException,IndexOutOfBoundsException

#### ? 谈谈你对解析与分派的认识。

说说你对Java反射的理解

- 说说你对Java注解的理解
- 说说你对依赖注入的理解
- 说一下泛型原理，并举例说明
- Java中String的了解
- String为什么要设计成不可变的？
- Object类的equal和hashCode方法重写，为什么？

### （三） 线程、多线程和线程池

#### ? 进程和线程的区别 协程呢

* 进程  

​	是系统给程序分配资源的基本单位，每个进程都有唯一的地址空间，一个程序至少有一个进程，一个进程至少有一个线程.

* 线程

  线程是执行操作的基本单位，JVM结构中共享 Method Area ,Heap Area

​	https://blog.csdn.net/mxsgoden/article/details/8821936

​	https://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html

进程 : 有很大的独立性

线程 :  所有线程都有完全一样的地址空间,意味着它们也共享同样的全局变量。由于线程可以访问进程地址空间的每一个内存地址，所以一个线程可以读、写甚至清除另一个线程的堆栈。

每个进程中的内容 : 地址空间  全局变量 打开文件 子进程	即将发生的定时器	信号与信号处理程序

每个线程中的内容：程序计数器、寄存器、堆栈、状态.



协程

​		协程拥有自己的寄存器上下文和栈，协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的	时候，恢复先前保存的寄存器上下文和栈，即所有局部状态的一个特定组合），每次过程重入时，就相当于进入上一次调用的状态，换种说法：进入上一次离开时所处逻辑流的位置。

协程的好处:

1. 无需线程上下文切换的开销 
2. 无需原子操作锁定及同步的开销
3. 方便切换控制流，简化编程模型

​	为什么要有线程，而不是仅仅用进程？



#### 如何控制某个方法允许并发访问线程的个数？

- 在Java中wait和seelp方法的不同；
- 谈谈wait/notify关键字的理解
- 什么导致线程阻塞？
- 线程如何关闭？
- 讲一下java中的同步的方法
- 数据一致性如何保证？
- 如何保证线程安全？
- 如何实现线程同步？
- 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
- 线程间操作List
- Java中对象的生命周期
- Synchronized用法
- synchronize的原理
- 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
- static synchronized 方法的多线程访问和作用
- 同一个类里面两个synchronized方法，两个线程同时访问的问题
- volatile的原理
- 谈谈volatile关键字的用法
- 谈谈volatile关键字的作用
- 谈谈NIO的理解
- synchronized 和volatile 关键字的区别
- synchronized与Lock的区别
- ReentrantLock 、synchronized和volatile比较
- ReentrantLock的内部实现
- lock原理
- 死锁的四个必要条件？
- 怎么避免死锁？
- 对象锁和类锁是否会互相影响？
- 什么是线程池，如何使用?
- Java的并发、多线程、线程模型
- 谈谈对多线程的理解
- 多线程有什么要注意的问题？
- 谈谈你对并发编程的理解并举例说明
- 谈谈你对多线程同步机制的理解？
- 如何保证多线程读写文件的安全？
- 多线程断点续传原理
- 断点续传的实现





### （四） 数据结构

- 常用数据结构简介

- 并发集合了解哪些？

- 列举java的集合以及集合之间的继承关系

- 集合类以及集合框架

- 容器类介绍以及之间的区别（容器类估计很多人没听这个词，Java容器主要可以划分为4个部分：List列表、Set集合、Map映射、工具类（Iterator迭代器、Enumeration枚举类、Arrays和Collections），具体的可以看看这篇博文 [Java容器类](http://alexyyek.github.io/2015/04/06/Collection/)）

  

- List,Set,Map的区别

- List和Map的实现方式以及存储方式

- 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？

- HashMap的实现原理

- HashMap数据结构？

- HashMap源码理解

- HashMap如何put数据（从HashMap源码角度讲解）？

- HashMap怎么手写实现？

  https://www.jianshu.com/p/985534b21089

- ConcurrentHashMap的实现原理

- ArrayMap和HashMap的对比

- HashTable实现原理

- TreeMap具体实现

- HashMap和HashTable的区别

- HashMap与HashSet的区别

- HashSet与HashMap怎么判断集合元素重复？

- 集合Set实现Hash怎么防止碰撞

- ArrayList和LinkedList的区别，以及应用场景

- 数组和链表的区别

- 二叉树的深度优先遍历和广度优先遍历的具体实现

- 堆的结构

- 堆和树的区别

- 堆和栈在内存中的区别是什么(解答提示：可以从数据结构方面以及实际实现方面两个方面去回答)？

- 什么是深拷贝和浅拷贝

- 手写链表逆序代码

- 讲一下对树，B+树的理解

- 讲一下对图的理解

- 判断单链表成环与否？

- 链表翻转（即：翻转一个单项链表）

- 合并多个单有序链表（假设都是递增的）



------



Kotin面试题

http://www.youkmi.cn/2019/10/27/kotlin-ti-mu-zheng-li/

https://www.jianshu.com/p/45866c8415c8



### （五）混合开发面试题

**大厂除了技术深度之外，还要求你具备一些广度的知识，比如你要会前端知识，会混合开发，至少会一种脚本语言，C c++更不用说了，也是必会的。**

- Hybrid做过吗？
- Hybrid通信原理是什么，有做研究吗？
- react native有多少了解？讲一下原理。
- weex了解吗？如何自己实现类似技术？
- flutter了解吗？内部是如何实现跨平台的？
- Dart语言有研究贵吗？
- 快应用了解吗？跟其她方式相比有什么优缺点？
- 说说你用过的混合开发技术有哪些？各有什么优缺点？
- Python会吗？
- 会不会PHP？
- Gradle了解多少？groovy语法会吗？
- 

------

### （六）高端技术面试题

**这里讲的是大公司需要用到的一些高端Android技术，这里专门整理了一个文档，希望大家都可以看看。这些题目有点技术含量，需要好点时间去研究一下的。**

##### （一）图片

- 图片库对比
- 图片库的源码分析
- 图片框架缓存实现
- LRUCache原理
- 图片加载原理
- 自己去实现图片库，怎么做？
- Glide源码解析
- Glide使用什么缓存？
- Glide内存缓存如何控制大小？

##### （二）网络和安全机制

- 网络框架对比和源码分析
- 自己去设计网络请求框架，怎么做？
- okhttp源码
- 网络请求缓存处理，okhttp如何处理网络缓存的
- 从网络加载一个10M的图片，说下注意事项
- TCP的3次握手和四次挥手
- TCP与UDP的区别
- TCP与UDP的应用
- HTTP协议
- HTTP1.0与2.0的区别
- HTTP报文结构
- HTTP与HTTPS的区别以及如何实现安全性
- 如何验证证书的合法性?
- https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
- client如何确定自己发送的消息被server收到?
- 谈谈你对WebSocket的理解
- WebSocket与socket的区别
- 谈谈你对安卓签名的理解。
- 请解释安卓为啥要加签名机制?
- 视频加密传输
- App 是如何沙箱化，为什么要这么做？
- 权限管理系统（底层的权限是如何进行 grant 的）？

##### （三）数据库

- sqlite升级，增加字段的语句
- 数据库框架对比和源码分析
- 数据库的优化
- 数据库数据迁移问题

##### （四）算法

- 排序算法有哪些？
- 最快的排序算法是哪个？
- 手写一个冒泡排序
- 手写快速排序代码
- 快速排序的过程、时间复杂度、空间复杂度
- 手写堆排序
- 堆排序过程、时间复杂度及空间复杂度
- 写出你所知道的排序算法及时空复杂度，稳定性
- 二叉树给出根节点和目标节点，找出从根节点到目标节点的路径
- 给阿里2万多名员工按年龄排序应该选择哪个算法？
- GC算法(各种算法的优缺点以及应用场景)
- 蚁群算法与蒙特卡洛算法
- 子串包含问题(KMP 算法)写代码实现
- 一个无序，不重复数组，输出N个元素，使得N个元素的和相加为M，给出时间复杂度、空间复杂度。手写算法
- 万亿级别的两个URL文件A和B，如何求出A和B的差集C(提示：Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)
- 百度POI中如何试下查找最近的商家功能(提示：坐标镜像+R树)。
- 两个不重复的数组集合中，求共同的元素。
- 两个不重复的数组集合中，这两个集合都是海量数据，内存中放不下，怎么求共同的元素？
- 一个文件中有100万个整数，由空格分开，在程序中判断用户输入的整数是否在此文件中。说出最优的方法
- 一张Bitmap所占内存以及内存占用的计算
- 2000万个整数，找出第五十大的数字？
- 烧一根不均匀的绳，从头烧到尾总共需要1个小时。现在有若干条材质相同的绳子，问如何用烧绳的方法来计时一个小时十五分钟呢？
- 求1000以内的水仙花数以及40亿以内的水仙花数
- 5枚硬币，2正3反如何划分为两堆然后通过翻转让两堆中正面向上的硬8币和反面向上的硬币个数相同
- 时针走一圈，时针分针重合几次
- N*N的方格纸,里面有多少个正方形
- x个苹果，一天只能吃一个、两个、或者三个，问多少天可以吃完？

##### （五）插件化、模块化、组件化、热修复、增量更新、Gradle

- 对热修复和插件化的理解
- 插件化原理分析
- 模块化实现（好处，原因）
- 热修复,插件化
- 项目组件化的理解
- 描述清点击 Android Studio 的 build 按钮后发生了什么

##### （六）架构设计和设计模式

- 谈谈你对Android设计模式的理解
- MVC MVP MVVM原理和区别
- 你所知道的设计模式有哪些？
- 项目中常用的设计模式
- 手写生产者/消费者模式
- 写出观察者模式的代码
- 适配器模式，装饰者模式，外观模式的异同？
- 用到的一些开源框架，介绍一个看过源码的，内部实现过程。
- 谈谈对RxJava的理解
- RxJava的功能与原理实现
- RxJava的作用，与平时使用的异步操作来比的优缺点
- 说说EventBus作用，实现方式，代替EventBus的方式
- 从0设计一款App整体架构，如何去做？
- 说一款你认为当前比较火的应用并设计(比如：直播APP，P2P金融，小视频等)
- 谈谈对java状态机理解
- Fragment如果在Adapter中使用应该如何解耦？
- Binder机制及底层实现
- 对于应用更新这块是如何做的？(解答：灰度，强制更新，分区域更新)？
- 实现一个Json解析器(可以通过正则提高速度)
- 统计启动时长,标准

##### （七）性能优化

- 如何对Android 应用进行性能分析以及优化?
- ddms 和 traceView
- 性能优化如何分析systrace？
- 用IDE如何分析内存泄漏？
- Java多线程引发的性能问题，怎么解决？
- 启动页白屏及黑屏解决？
- 启动太慢怎么解决？
- 怎么保证应用启动不卡顿？
- App启动崩溃异常捕捉
- 自定义View注意事项
- 现在下载速度很慢,试从网络协议的角度分析原因,并优化(提示：网络的5层都可以涉及)。
- Https请求慢的解决办法（提示：DNS，携带数据，直接访问IP）
- 如何保持应用的稳定性
- RecyclerView和ListView的性能对比
- ListView的优化
- RecycleView优化
- View渲染
- Bitmap如何处理大图，如一张30M的大图，如何预防OOM
- java中的四种引用的区别以及使用场景
- 强引用置为null，会不会被回收？

##### （八）NDK、jni、Binder、AIDL、进程通信有关

- 请介绍一下NDK
- 什么是NDK库?
- jni用过吗？
- 如何在jni中注册native函数，有几种注册方式?
- Java如何调用c、c++语言？
- jni如何调用java层代码？
- 进程间通信的方式？
- Binder机制
- 简述IPC？
- 什么是AIDL？
- AIDL解决了什么问题？
- AIDL如何使用？
- Android 上的 Inter-Process-Communication 跨进程通信时如何工作的？
- 多进程场景遇见过么？
- Android进程分类？
- 进程和 Application 的生命周期？
- 进程调度
- 谈谈对进程共享和线程安全的认识
- 谈谈对多进程开发的理解以及多进程应用场景
- 什么是协程？

##### （九）framework层、ROM定制、Ubuntu、Linux之类的问题

- java虚拟机的特性
- 谈谈对jvm的理解
- JVM内存区域，开线程影响哪块内存
- 对Dalvik、ART虚拟机有什么了解？
- Art和Dalvik对比
- 虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)
- 谈谈你对双亲委派模型理解
- JVM内存模型，内存区域
- 类加载机制
- 谈谈对ClassLoader(类加载器)的理解
- 谈谈对动态加载（OSGI）的理解
- 内存对象的循环引用及避免
- 内存回收机制、GC回收策略、GC原理时机以及GC对象
- 垃圾回收机制与调用System.gc()区别
- Ubuntu编译安卓系统
- 系统启动流程是什么？（提示：Zygote进程 –> SystemServer进程 –> 各种系统服务 –> 应用进程）
- 大体说清一个应用程序安装到手机上时发生了什么
- 简述Activity启动全部过程
- App启动流程，从点击桌面开始
- 逻辑地址与物理地址，为什么使用逻辑地址？
- Android为每个应用程序分配的内存大小是多少？
- Android中进程内存的分配，能不能自己分配定额内存？
- 进程保活的方式
- 如何保证一个后台服务不被杀死？（相同问题：如何保证service在后台不被kill？）比较省电的方式是什么？
- App中唤醒其他进程的实现方式



​	https://www.jianshu.com/p/c3965e82b164

- https://www.cnblogs.com/deman/p/5860976.html#_label29)

