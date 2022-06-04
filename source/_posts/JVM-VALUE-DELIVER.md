---
title: JVM-VALUE-DELIVER
date: 2022-05-05 12:46:27
tags: JVM
categories: JAVA
---



再做leetcode时也遇到了，java时引用传递还是值传递的问题，这方面 指针就很清楚,

https://segmentfault.com/a/1190000016773324 这里讲解很清楚

> **1. 基本数据类型的存储：**

- A. 基本数据类型的局部变量
- B. 基本数据类型的成员变量
- C. 基本数据类型的静态变量

> **2. 引用数据类型的存储**





##### **A.基本数据类型的局部变量**

1. 定义基本数据类型的局部变量以及数据都是直接存储在内存中的栈上，也就是前面说到的**“虚拟机栈”**，数据本身的值就是存储在栈空间里面。



![](https://segmentfault.com/img/remote/1460000016773335?w=1352&h=584)





```java
public class JavaValue {
    //测试
    public static void main(String[] args) {
        Person p = new Person();
        p.setName("我是马化腾");
        p.setAge(45);
        PersonCrossTest(p);
        System.out.println("方法执行后的name：" + p.getName());
    }

    public static void PersonCrossTest(Person person) {
        System.out.println("Before new Person 传入的person的name：" + person.getName());
        person = new Person();
        System.out.println("After new Person 传入的person的name：" + person.getName());
        person.setName("我是张小龙");
        System.out.println("方法内重新赋值后的name：" + person.getName());
    }
}
```



person = new Person();一旦执行这一步操作后，把之前的指向就去掉了
