---
title: DesignPattern_Decorator
comments: true
date: 2020-07-12 12:37:59
tags:
categories: DesignPattern
---

**装饰器模式**

应用场景: 拓展一个类的功能或给一个类添加附加职责.

优点: 

1. 不改变原有对象的情况下给一个对象拓展功能
2. 使用不同的组合实现不同的效果
3. 符合开闭原则

![](DesignPattern-Decorator/2021-07-12_decorator.png)

```java
interface Component{
    void operation();
}

// 具体类
class ConcreteComponent implements  Component{
    @Override
    public void operation() {
        System.out.println("拍照");
    }
}

//对具体类的拓展
class ConcreteComponent1 implements  Component {

    private Component component;
    public ConcreteComponent1(Component component) {
        this.component = component;
    }

    @Override
    public void operation() {
        System.out.println("美颜");
        component.operation();
    }
}

//测试
new ConcreteComponent1(new ConcreteComponent()).operation();
```

也可以把装饰器抽象出来

```java
interface Component{
    void operation();
}
// 具体类
class ConcreateComponent implements  Component{
    @Override
    public void operation() {
        System.out.println("拍照");
    }
}

abstract class Decorator implements Component{
    Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void operation() {
        component.operation();
    }
}

//对具体类的拓展
class ConcreateComponent1 extends  Decorator{

    public ConcreateComponent1(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        System.out.println("美颜");
        super.operation();
    }
}

class ConcreateComponent2 extends  Decorator{

    public ConcreateComponent2(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        System.out.println("呵呵");
        super.operation();
    }
}

//测试
 ConcreateComponent2 concreateComponent1 = new ConcreateComponent2(new ConcreateComponent1(new       ConcreateComponent()));
        concreateComponent1.operation();
```

https://www.bilibili.com/video/BV1hp4y1D7MP?from=search&seid=6785978722248254154

https://www.bilibili.com/video/BV1Hv411r7uN?p=2