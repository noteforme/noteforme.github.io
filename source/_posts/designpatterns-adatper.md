---
id: 463
title: 'DesignPatterns Adatper'
date: '2024-06-13T01:36:16+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=463'
permalink: /2024/06/13/designpatterns-adatper/
categories:
    - DesignPattern
---



The Adapter Pattern can be implemented in two main ways: Class Adapter and Object Adapter. Both serve the purpose of adapting one interface to another, but they do so using different approaches.

# Object Adapter

An Object Adapter uses composition to adapt one interface to another. This approach involves holding an instance of the Adaptee within the Adapter and delegating calls to it.

Example of Object Adapter in Kotlin:

1. Target Interface: Defines the expected interface.
2. Adaptee: The existing class with an incompatible interface.
3. Object Adapter: Uses composition to delegate calls to the Adaptee.

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/Architecture/object-adapter.png)

```java
// Target interface
public interface Target {
    void request();
}

// Adaptee class
public class Adaptee {
    public void specificRequest() {
        System.out.println("Called specificRequest()");
    }
}

// Object Adapter
public class ObjectAdapter implements Target {
    private Adaptee adaptee;

    public ObjectAdapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new ObjectAdapter(adaptee);
        target.request(); // Output: Called specificRequest()
    }
}
```

# Class Adapter

A Class Adapter uses inheritance to adapt one interface to another. This approach requires multiple inheritance (which Kotlin supports through interfaces).

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/Architecture/class-adapter.jpg)

Example of Class Adapter in Kotlin:

1. Target Interface: Defines the expected interface.
2. Adaptee: The existing class with an incompatible interface.
3. Class Adapter: Inherits from both the Target Interface and the Adaptee.

```java
// Target Interface
interface NewPaymentSystem {
    fun pay(amount: Double)
}

// Adaptee
class OldPaymentSystem {
    fun makePayment(amount: Double) {
        println("Payment of \$$amount made using Old Payment System")
    }
}

// Class Adapter
class PaymentClassAdapter : OldPaymentSystem(), NewPaymentSystem {
    override fun pay(amount: Double) {
        makePayment(amount)
    }
}

// Client Code
fun main() {
    val adapter = PaymentClassAdapter()
    adapter.pay(100.0)  // Outputs: Payment of $100.0 made using Old Payment System
}
```

# Differences Between Class Adapter and Object Adapter:

Inheritance vs. Composition:

Class Adapter uses inheritance to adapt the Adaptee to the Target Interface.
Object Adapter uses composition to hold an instance of the Adaptee and delegate calls to it.
Flexibility:

Class Adapter is less flexible because it requires multiple inheritance, which can lead to complications if the Adaptee class changes or if you need to adapt multiple classes.
Object Adapter is more flexible as it uses composition, making it easier to adapt multiple classes and change the Adaptee without affecting the Adapter.
Reuse:

Class Adapter reuses the Adaptee class directly through inheritance.
Object Adapter reuses the Adaptee class indirectly through composition, which can be more reusable in different contexts.

    类适配器使用对象继承的方式，是静态的定义方式；
    
    对象适配器使用对象组合的方式，是动态组合的方式。
    
    对于类适配器，由于适配器直接继承了Adaptee，使得适配器不能和Adaptee的子类一起工作，因为继承是静态的关系，当适配器继承了Adaptee后，就不可能再去处理 Adaptee的子类了。
    
    对于对象适配器，一个适配器可以把多种不同的源适配到同一个目标。换言之，同一个适配器可以把源类和它的子类都适配到目标接口。因为对象适配器采用的是对象组合的关系，只要对象类型正确，是不是子类都无所谓。
    
    对于类适配器，适配器可以重定义Adaptee的部分行为，相当于子类覆盖父类的部分实现方法。
    
    对于对象适配器，要重定义Adaptee的行为比较困难，这种情况下，需要定义Adaptee的子类来实现重定义，然后让适配器组合子类。虽然重定义Adaptee的行为比较困难，但是想要增加一些新的行为则方便的很，而且新增加的行为可同时适用于所有的源。
    
    对于类适配器，仅仅引入了一个对象，并不需要额外的引用来间接得到Adaptee。
    
    对于对象适配器，需要额外的引用来间接得到Adaptee。

建议尽量使用对象适配器的实现方式，多用合成/聚合、少用继承。当然，具体问题具体分析，根据需要来选用实现方式，最适合的才是最好的。

参考　：
https://github.com/stven0king/designmode?tab=readme-ov-file

https://github.com/helen-x/AndroidInterview/blob/master/android/Android%20%E6%BA%90%E7%A0%81%E4%B8%AD%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F(%E4%BD%A0%E9%9C%80%E8%A6%81%E7%9F%A5%E9%81%93%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E5%85%A8%E5%9C%A8%E8%BF%99%E9%87%8C).md