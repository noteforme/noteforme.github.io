---
title: JVM_Overview
comments: true
date: 2020-01-03 10:48:45
tags: JVM
categories: JAVA
---





Mark word



虚拟机是通过软件模拟的具有完整硬件系统功能的、运行在一个完全隔离环境中的完整计算机系统。

JVM分类

https://www.zhihu.com/question/29265430/answer/43818804

元空间寸哪些数据?

https://www.cnblogs.com/duanxz/p/3520829.html

JVM整体结构



![j_20220607223724](JVM-Overview/j_20220607223724.jpg)



![](JVM-Overview/j_image-20200704183436495.png)

Java代码执行流程

![](JVM-Overview/j_image-20200704210429535.png)



​	详细图

![](JVM-Overview/j_jvm0002.jpg)

## JVM的架构模型

Java编译器输入的指令流基本上是一种基于栈的指令集架构，另外一种指令集架构则是基于寄存器的指令集架构。具体来说：这两种架构之间的区别：

基于栈式架构的特点

- 设计和实现更简单，适用于资源受限的系统；
- 避开了寄存器的分配难题：使用零地址指令方式分配。
- 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现。
- 不需要硬件支持，可移植性更好，更好实现跨平台

基于寄存器架构的特点

- 典型的应用是x86的二进制指令集：比如传统的PC以及Android的Davlik虚拟机。
- 指令集架构则完全依赖硬件，可移植性差
- 性能优秀和执行更高效
- 花费更少的指令去完成一项操作。
- 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主方水洋

总结

由于跨平台性的设计，Java的指令都是根据栈来设计的。不同平台CPU架构不同，所以不能设计为基于寄存器的。优点是跨平台，指令集小，编译器容易实现，缺点是性能下降，实现同样的功能需要更多的指令。

时至今日，尽管嵌入式平台已经不是Java程序的主流运行平台了（准确来说应该是HotSpotVM的宿主环境已经不局限于嵌入式平台了），那么为什么不将架构更换为基于寄存器的架构呢？

栈

- 跨平台性
- 指令集小
- 指令多
- 执行性能比寄存器差

## [类加载过程](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=类加载过程)

```java
public class HelloLoader {

    public static void main(String[] args) {
        System.out.println("谢谢ClassLoader加载我....");
        System.out.println("你的大恩大德，我下辈子再报！");
    }
}
```

它的加载过程是怎么样的呢?

- 执行 main() 方法（静态方法）就需要先加载main方法所在类 HelloLoader
- 加载成功，则进行链接、初始化等操作。完成后调用 HelloLoader 类中的静态方法 main
- 加载失败则抛出异常

![](JVM-Overview/j_jvm0006.png)

完整的流程图如下所示：

![](JVM-Overview/j_0007.png)

### [加载阶段](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=加载阶段)

**加载：**

1. 通过一个类的全限定名获取定义此类的二进制字节流
2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构
3. **在内存中生成一个代表这个类的java.lang.Class对象**，作为方法区这个类的各种数据的访问入口

**加载class文件的方式：**

1. 从本地系统中直接加载
2. 通过网络获取，典型场景：Web Applet
3. 从zip压缩包中读取，成为日后jar、war格式的基础
4. 运行时计算生成，使用最多的是：动态代理技术
5. 由其他文件生成，典型场景：JSP应用从专有数据库中提取.class文件，比较少见
6. 从加密文件中获取，典型的防Class文件被反编译的保护措施

### [链接阶段](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=链接阶段)

链接分为三个子阶段：验证 -> 准备 -> 解析

1. 目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全
2. 主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证。

**举例**

使用 BinaryViewer软件查看字节码文件，其开头均为 CAFE BABE ，如果出现不合法的字节码文件，那么将会验证不通过。

#### [准备(Prepare)](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=准备prepare)

1. 为类变量（static变量）分配内存并且设置该类变量的默认初始值，即零值
2. 这里不包含用final修饰的static，因为final在编译的时候就会分配好了默认值，准备阶段会显式初始化
3. 注意：这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到Java堆中

代码：变量a在准备阶段会赋初始值，但不是1，而是0，在初始化阶段会被赋值为 1

```java
public class HelloApp {
    private static int a = 1;//prepare：a = 0 ---> initial : a = 1


    public static void main(String[] args) {
        System.out.println(a);
    }
}
```

#### [解析(Resolve)](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=解析resolve)

1. **将常量池内的符号引用转换为直接引用的过程**
2. 事实上，解析操作往往会伴随着JVM在执行完初始化之后再执行
3. 符号引用就是一组符号来描述所引用的目标。符号引用的字面量形式明确定义在《java虚拟机规范》的class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄
4. 解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT Class info、CONSTANT Fieldref info、CONSTANT Methodref info等

**符号引用**

- 反编译 class 文件后可以查看符号引用

### [初始化阶段](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=初始化阶段)

#### [类的初始化时机](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=类的初始化时机)

1. 创建类的实例
2. 访问某个类或接口的静态变量，或者对该静态变量赋值
3. 调用类的静态方法
4. 反射（比如：Class.forName(“com.atguigu.Test”)）
5. 初始化一个类的子类
6. Java虚拟机启动时被标明为启动类的类
7. JDK7开始提供的动态语言支持：java.lang.invoke.MethodHandle实例的解析结果REF_getStatic、REF putStatic、REF_invokeStatic句柄对应的类没有初始化，则初始化

除了以上七种情况，其他使用Java类的方式都被看作是对类的被动使用，都不会导致类的初始化，即不会执行初始化阶段（不会调用 clinit() 方法和 init() 方法）

### [clinit()](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=clinit)

1. 初始化阶段就是执行类构造器方法`<clinit>()`的过程
2. 此方法不需定义，是javac编译器自动收集类中的所有**类变量**的赋值动作和静态代码块中的语句合并而来。也就是说，当我们代码中包含static变量的时候，就会有clinit方法
3. `<clinit>()`方法中的指令按语句在源文件中出现的顺序执行
4. `<clinit>()`不同于类的构造器。（关联：构造器是虚拟机视角下的`<init>()`）
5. 若该类具有父类，JVM会保证子类的`<clinit>()`执行前，父类的`<clinit>()`已经执行完毕
6. 虚拟机必须保证一个类的`<clinit>()`方法在多线程下被同步加锁

> IDEA 中安装 JClassLib Bytecode viewer 插件，可以很方便的看字节码。安装过程可以自行百度

![](JVM-Overview/0010.png)

#### [5说明](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=_5说明)

若该类具有父类，JVM会保证子类的`<clinit>()`执行前，父类的`<clinit>()`已经执行完毕

![](JVM-Overview/j_0013.png)

如上代码，加载流程如下：

- 首先，执行 main() 方法需要加载 ClinitTest1 类
- 获取 Son.B 静态变量，需要加载 Son 类
- Son 类的父类是 Father 类，所以需要先执行 Father 类的加载，再执行 Son 类的加载

#### [6说明](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=_6说明)

虚拟机必须保证一个类的`<clinit>()`方法在多线程下被同步加锁

```java
public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            DeadThread dead = new DeadThread();
            System.out.println(Thread.currentThread().getName() + "结束");
        };

        Thread t1 = new Thread(r,"线程1");
        Thread t2 = new Thread(r,"线程2");

        t1.start();
        t2.start();
    }
}

class DeadThread{
    static{
        if(true){
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
            while(true){

            }
        }
    }
}
```

```
线程2开始
线程1开始
线程2初始化当前类

/然后程序卡死了
```

程序卡死，分析原因：

- 两个线程同时去加载 DeadThread 类，而 DeadThread 类中静态代码块中有一处死循环
- 先加载 DeadThread 类的线程抢到了同步锁，然后在类的静态代码块中执行死循环，而另一个线程在等待同步锁的释放
- 所以无论哪个线程先执行 DeadThread 类的加载，另外一个类也不会继续执行。（一个类只会被加载一次）

## [类加载器的分类](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=类加载器的分类)

### [概述](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=概述-1)

1. JVM严格来讲支持两种类型的类加载器 。分别为引导类加载器（Bootstrap ClassLoader）和自定义类加载器（User-Defined ClassLoader）
2. 从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是**将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器**
3. 无论类加载器的类型如何划分，在程序中我们最常见的类加载器始终只有3个，如下所示

![](JVM-Overview/j_0014.png)

```java
public class ClassLoaderTest {
    public static void main(String[] args) {

        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader);//sun.misc.Launcher$ExtClassLoader@1540e19d

        //获取其上层：获取不到引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader);//null

        //对于用户自定义类来说：默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //String类使用引导类加载器进行加载的。---> Java的核心类库都是使用引导类加载器进行加载的。
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1);//null


    }
}
```

- 我们尝试获取引导类加载器，获取到的值为 null ，这并不代表引导类加载器不存在，**因为引导类加载器右 C/C++ 语言，我们获取不到**
- 两次获取系统类加载器的值都相同：sun.misc.Launcher$AppClassLoader@18b4aac2 ，这说明**系统类加载器是全局唯一的**

## [双亲委派机制](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=双亲委派机制)

### [双亲委派机制原理](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=双亲委派机制原理)

ava虚拟机对class文件采用的是**按需加载**的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，Java虚拟机采用的是双亲委派模式，即把请求交由父类处理，它是一种任务委派模式

1. 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行；

2. 如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器；

3. 如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式。

4. 父类加载器一层一层往下分配任务，如果子类加载器能加载，则加载此类，如果将加载任务分配至系统类加载器也无法加载此类，则抛出异常

   ![](JVM-Overview/j_0020.png)

### [双亲委派机制代码演示](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=双亲委派机制代码演示)

#### [举例1](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=举例1)

1、我们自己建立一个 java.lang.String 类，写上 static 代码块

```java
public class String {
    //
    static{
        System.out.println("我是自定义的String类的静态代码块");
    }
}
```

2、在另外的程序中加载 String 类，看看加载的 String 类是 JDK 自带的 String 类，还是我们自己编写的 String 类

```java
public class StringTest {

    public static void main(String[] args) {
        java.lang.String str = new java.lang.String();
        System.out.println("hello,atguigu.com");

        StringTest test = new StringTest();
        System.out.println(test.getClass().getClassLoader());
    }
}
```

```
hello,atguigu.com
sun.misc.Launcher$AppClassLoader@18b4aac2
```

程序并没有输出我们静态代码块中的内容，可见仍然加载的是 JDK 自带的 String 类。

把刚刚的类改一下

```java
package java.lang;
public class String {
    //
    static{
        System.out.println("我是自定义的String类的静态代码块");
    }
    //错误: 在类 java.lang.String 中找不到 main 方法
    public static void main(String[] args) {
        System.out.println("hello,String");
    }
}
```

![](JVM-Overview/j_0021.png)

由于双亲委派机制一直找父类，所以最后找到了Bootstrap ClassLoader，Bootstrap ClassLoader找到的是 JDK 自带的 String 类，在那个String类中并没有 main() 方法，所以就报了上面的错误。

```java
package java.lang;


public class ShkStart {

    public static void main(String[] args) {
        System.out.println("hello!");
    }
}
```

```java
java.lang.SecurityException: Prohibited package name: java.lang
    at java.lang.ClassLoader.preDefineClass(ClassLoader.java:662)
    at java.lang.ClassLoader.defineClass(ClassLoader.java:761)
    at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
    at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
    at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
    at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
    at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
    at java.security.AccessController.doPrivileged(Native Method)
    at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
    at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
    at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:335)
    at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
    at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:495)
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" 
Process finished with exit code 1
```

即使类名没有重复，也禁止使用java.lang这种包名。这是一种保护机制

#### [举例3](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=举例3)

当我们加载jdbc.jar 用于实现数据库连接的时候

1. 我们现在程序中需要用到SPI接口，而SPI接口属于rt.jar包中Java核心api
2. 然后使用双清委派机制，引导类加载器把rt.jar包加载进来，而rt.jar包中的SPI存在一些接口，接口我们就需要具体的实现类了
3. 具体的实现类就涉及到了某些第三方的jar包了，比如我们加载SPI的实现类jdbc.jar包【首先我们需要知道的是 jdbc.jar是基于SPI接口进行实现的】
4. 第三方的jar包中的类属于系统类加载器来加载
5. 从这里面就可以看到SPI核心接口由引导类加载器来加载，SPI具体实现类由系统类加载器来加载

![](JVM-Overview/j_0022.png)

### [双亲委派机制优势](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=双亲委派机制优势)

通过上面的例子，我们可以知道，双亲机制可以

1. 避免类的重复加载

2. 保护程序安全，防止核心API被随意篡改

   - 自定义类：自定义java.lang.String 没有被加载。

   - 自定义类：java.lang.ShkStart（报错：阻止创建 java.lang开头的类）

     

## [沙箱安全机制](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=沙箱安全机制)

1. 自定义String类时：在加载自定义String类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载jdk自带的文件（rt.jar包中java.lang.String.class），报错信息说没有main方法，就是因为加载的是rt.jar包中的String类。
2. 这样可以保证对java核心源代码的保护，这就是沙箱安全机制。

## [其他](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=其他)

### [如何判断两个class对象是否相同？](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=如何判断两个class对象是否相同？)

在JVM中表示两个class对象是否为同一个类存在两个必要条件：

1. 类的完整类名必须一致，包括包名
2. **加载这个类的ClassLoader（指ClassLoader实例对象）必须相同**
3. 换句话说，在JVM中，即使这两个类对象（class对象）来源同一个Class文件，被同一个虚拟机所加载，但只要加载它们的ClassLoader实例对象不同，那么这两个类对象也是不相等的

### [对类加载器的引用](https://youthlql.gitee.io/javayouth/#/docs/Java/JVM/JVM系列-第2章-类加载子系统?id=对类加载器的引用)

1. JVM必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的
2. **如果一个类型是由用户类加载器加载的，那么JVM会将这个类加载器的一个引用作为类型信息的一部分保存在方法区中**
3. 当解析一个类型到另一个类型的引用的时候，JVM需要保证这两个类型的类加载器是相同的（后面讲）



https://youthlql.gitee.io/javayouth/#/?id=java