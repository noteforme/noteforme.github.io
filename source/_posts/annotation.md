---
id: 354
title: Annotation
date: '2024-06-06T02:13:46+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=354'
permalink: /2024/06/06/annotation/
categories:
    - JAVA
---



Annotations are used to provide supplementary information about a program and can be used for a variety of purposes, such as:

1. Information for the compiler: Annotations can be used by the compiler to detect errors or suppress warnings.
2. Compile-time processing: Software tools can process annotation information to generate code, XML files, etc.
3. Runtime processing: Some annotations are available to be examined at runtime via reflection.
   https://docs.oracle.com/javase%2Ftutorial%2F/java/annotations/index.html

# 注解

## 注解的作用

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-07-29_9.08.16_function.png)

- 提供信息给编译器： 编译器可以利用注解来探测错误和警告信息.
- 编译阶段时的处理：  软件工具可以用来利用注解信息来生成代码、Html文档或者做其它相应处理。
- 运行时的处理：  某些注解可以在程序运行的时候接受代码的提取.

元注解 ： 不可以再分的注解， 负责注解其他注解。java定义了4个标准的meta-annotation类型,它们被用来提供对其它annotation类型作说明。Target Retention Documented Inherited

如上图的 Overide被@Target和@Retention修饰，他们用来说明解释其他注解，位于 java.lang.annotation下

# Annotations That Apply to Other Annotations

Annotations that apply to other annotations are called meta-annotations. There are several meta-annotation types defined in java.lang.annotation.

### @Retention

 @Retention annotation specifies how the marked annotation is stored:

需要什么级别保存该注释信息，用于保留注解的生命周期

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-06-28_21-36-57_retention.png)

#### RetentionPolicy.SOURCE

 The marked annotation is retained only in the source level and is ignored by the compiler.

#### RetentionPolicy.CLASS

 – The marked annotation is retained by the compiler at compile time, but is ignored by the Java Virtual Machine (JVM).
 RetentionPolicy.RUNTIME – The marked annotation is retained by the JVM so it can be used by the runtime environment.

#### RetentionPolicy.RUNTIME

 The marked annotation is retained by the JVM so it can be used by the runtime environment.

# 编译和运行时注解区别

## 运行时注解

运行时注解的实质是，在代码中通过注解进行标记，运行时通过反射寻找标记进行某种处理。而运行时注解一直以来被呕病的原因便是反射的低效。
https://juejin.cn/post/6925283352698159117#heading-4

注解设置 SxtStudent信息，映射数据库

```java
@Target(value = {ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface SxtTable {
    String value();
}
```

```java
@Target(value = {ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface SxtField {
    String columnName();
    String type();
    int length();
}
```

```java
@SxtTable("tb_student")
public class SxtStudent {
    @SxtField(columnName = "id",type = "int",length = 10)
    private  int id;

    @SxtField(columnName = "sname",type = "varchar",length = 10)
    private String studentName;

    @SxtField(columnName = "age",type = "int",length = 3)
    private int age;
}
```

通过第三方工具获取注解信息

```java
try {
    Class clazz = Class.forName("annotation.SxtStudent");

    //获得类的所有有效注解
    Annotation[] annotations = clazz.getAnnotations();
    for (Annotation a:annotations){
        System.out.println(a);
    }
    //获得类的指定注解
    SxtTable st = (SxtTable)clazz.getAnnotation(SxtTable.class);
    System.out.println(st.value());

    //获得类的属性的注解
    Field f = clazz.getDeclaredField("studentName");
    SxtField sxtField = f.getAnnotation(SxtField.class);
    System.out.println(sxtField.columnName()+"--" + sxtField.type()+"---" + sxtField.length());

} catch (ClassNotFoundException e) {
    e.printStackTrace();
} catch (NoSuchFieldException e) {
    e.printStackTrace();
}
```

运行结果

> @annotation.SxtTable(value=tb_student)
> tb_student
> sname--varchar---10

运行时注解很好理解，代码运行的时候通过反射技术就可以获取上面的注解信息。从上面可以看到,反射增加了点性能问题

### 反射实例

```java
String path = "reflection.User";
try {
    Class<?> clazz = Class.forName(path);
    //对象是表示或封装一些数据。一个类被加载后，JVM会创建一个对应该类的Class对象，类的整个结构信息会放到对应的CLass对象中。
    //这个Class对象就像一面镜子一样，通过这面镜子我可已看到对应类的全部信息。
    System.out.println(clazz.hashCode());

    Class clazz2 = Class.forName(path); // 一个类对应的一个class对象
    System.out.println(clazz2.hashCode());

    Class intCLazz = int.class;
    int[] arr01 = new int[10];
    int[][] arr02 = new int[30][3];
    int[]arr03 = new int[30];
    double[] arr04 = new double[10];

    System.out.println(arr01.getClass().hashCode());
    System.out.println(arr02.getClass().hashCode());
    System.out.println(arr03.getClass().hashCode());
    System.out.println(arr04.getClass().hashCode());
```

https://www.youtube.com/watch?v=vZe5zG0vh3U&list=PLC664nq_h8b9Jzh-qYJ_AeOpmSoBLC_tg&index=1

## 编译时注解

APT(Annotation Processing Tool)即注解处理器（通常也叫做编译时注解、编译时代码自动生成），是一种处理注解的工具，确切的说它是javac的一个工具，它用来在编译时扫描和处理注解。注解处理器以Java代码(或者编译过的字节码)作为输入，生成.java文件作为输出。
简单来说就是在编译期，通过注解生成.java文件。
通过上面的讲解，我们知道了：通常编译时注解要结合注解处理器一起使用的，通过解析注解，获取注解上面的信息，然后生成代码，从而生成一下辅助我们自己手写的代码。说得有点抽象，下面通过一个简单示例来演示一下。

我们做微信相关api，会有下面操作

示例最终效果如下：

```java
@SubTypeAutoGenerate("com.flyme.videoclips.wxapi.WXEntryActivity")
public class BaseWXEntryActivity extends Activity implements IWXAPIEventHandler {}

// 自动生成的代码,请不要改动
package com.flyme.videoclips.wxapi;

import com.flyme.videoclips.util.wxapi.BaseWXEntryActivity;

public class WXEntryActivity extends BaseWXEntryActivity {
}
```

我们通过一个@SubTypeAutoGenerate注解就可以很方便地生成WXEntryActivity啦，其中@SubTypeAutoGenerate注解的参数就是生成的全类名。
reference: https://juejin.cn/post/6925283352698159117

根据前面注解分类，编译时注解是保留到编译阶段的，即.class文件，不会保留到dex里面，即运行时根本获取不到这个注解了，那么这种编译时注解又有什么用呢？

## 注解处理器

是javac处理注解的一种工具，它用来在编译时扫描和处理注解。简单来说就是在编译器，通过注解采集信息，生成.java文件。减少重复代码的编写。

### APT原理

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-07-29_2.39.54_apt.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-07-29_2.10_process.png)

### 步骤

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-07-29_2.10.48_step.png)

### 问题

APT就这固定的几步，网上demo可以生成，我的就是没法生成，原来是因为,@DIActivity类的头部没有设置

```java
@DIActivity
public class MainActivity extends AppCompatActivity {
}
```

项目 MRouter

# Arouter

## 原理

获得当前程序在手机中对应的apk文件,使用classsloader:dexfile(PMS)，反射获取apk所有的类，筛选出注册activity的类。初始化

## 核心实现

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-08-12_1.18_arouter.png)

https://www.bilibili.com/video/BV1Ly4y1W7f5?p=6&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1nU4y1a79o?from=search&seid=3188418579827888379

https://juejin.cn/post/6925283352698159117#heading-14

https://juejin.cn/post/6947992544252788767

https://blog.csdn.net/xx326664162/article/details/68490059

https://hanshuliang.blog.csdn.net/article/details/117072224

https://github.com/han1202012/APT

### @Target

@Target annotation marks another annotation to restrict what kind of Java elements the annotation can be applied to. A target annotation specifies one of the following element types as its value:

ElementType.ANNOTATION_TYPE can be applied to an annotation type.
ElementType.CONSTRUCTOR can be applied to a constructor.
ElementType.FIELD can be applied to a field or property.
ElementType.LOCAL_VARIABLE can be applied to a local variable.
ElementType.METHOD can be applied to a method-level annotation.
ElementType.PACKAGE can be applied to a package declaration.
ElementType.PARAMETER can be applied to the parameters of a method.
ElementType.TYPE can be applied to any element of a class.

用于描述注解的使用范围(注解可以用在什么地方)
如 类 属性 方法等
![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/annotation/2021-06-28_21-32-34_target.png)

### @Inherited

@Inherited annotation indicates that the annotation type can be inherited from the super class. (This is not true by default.) When the user queries the annotation type and the class has no annotation for this type, the class' superclass is queried for the annotation type. This annotation applies only to class declarations.

标明所修饰的注解，在所作用的类上，是否可以被继承。

```java
@InheritedAnnotation(value = "Applied to SuperClass")
class SuperClass {
    // Superclass implementation
}

class SubClass extends SuperClass {
    // Subclass implementation
}

public class InheritedTest {
    public static void main(String[] args) {
        Class<SubClass> subClass = SubClass.class;
        // Check if the subclass inherits the annotation
        if (subClass.isAnnotationPresent(InheritedAnnotation.class)) {
            Annotation annotation = subClass.getAnnotation(InheritedAnnotation.class);
            InheritedAnnotation inheritedAnnotation = (InheritedAnnotation) annotation;
            System.out.println("SubClass inherits annotation: " + inheritedAnnotation.value());
        } else {
            System.out.println("SubClass does not inherit the annotation.");
        }
    }
}
```

output:
SubClass inherits annotation: Applied to SuperClas , 所以可以拿到父类的注解信息。

### @Repeatable

@Repeatable annotation, introduced in Java SE 8, indicates that the marked annotation can be applied more than once to the same declaration or type use. For more information, see Repeating Annotations.

```java
@Repeatable(ReviewTags.class)
@Retention(RetentionPolicy.RUNTIME)
@interface ReviewTag {
    String value();
}

@Retention(RetentionPolicy.RUNTIME)
@interface ReviewTags {
    ReviewTag[] value();
}

    public static void main(String[] args) {
        try {
            // Accessing annotations
            ReviewTag[] tags = ReviewTagsTest.class.getMethod("review").getAnnotationsByType(ReviewTag.class);
            System.out.println("Review Tags:");
            for (ReviewTag tag : tags) {
                System.out.println(tag.value());
            }
        } catch (NoSuchMethodException e) {
        }
    }
```

output : 

```bash
Positive
Detailed
```

### @Documented

@Documented annotation indicates that whenever the specified annotation is used those elements should be documented using the Javadoc tool. (By default, annotations are not included in Javadoc.) For more information, see the Javadoc tools page.
javadoc的工具文档化
 Declaring an Annotation Type ,Generate Document 
ClassPreamble.java

```java
package annotation.doc;

import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.lang.annotation.ElementType;

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface ClassPreamble {
    String author();
    String date();
    int currentRevision() default 1;
    String lastModified() default "N/A";
    String lastModifiedBy() default "N/A";
    String[] reviewers();
}
```

MyClass.java

```java
package annotation.doc;

@ClassPreamble(
    author = "John Doe",
    date = "06/06/2024",
    currentRevision = 2,
    lastModified = "06/06/2024",
    lastModifiedBy = "Jane Doe",
    reviewers = {"Alice", "Bob"}
)
public class MyClass {
    // Class implementation
    void print(){ // the function will not generate doc
        System.out.println("print");
    }
}
```

command steps

```bash
$ pwd
/Users/m/Documents/workpersonal/THINKKOTLIN/src/main/java
$ javac  annotation/doc/ClassPreamble.java  annotation/doc/MyClass.java
$ javadoc  -d doc  annotation/doc/MyClass.java
```

这种方式可以生成文档