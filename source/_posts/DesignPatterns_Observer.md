---
title: DesignPatterns_Observer
comments: true
date: 2018-06-06 16:09:58
tags:
categories: DesignPattern
---

#### 原理图

![](https://user-gold-cdn.xitu.io/2020/3/23/171064de21df41e0?imageView2/0/w/1280/h/960/ignore-error/1)

​                    奶站\天气站                                                                                                                           用户

#### Observer观察者模式/订阅模式

* PUSH 模式

```java
//主题对象变化，通知所有的观察者
//抽象观察者
public interface Observer{
    public void update(String str);
}
//具体观察者
public class ConcreteObserver implements Observer{
    @Override
    public void update(String str) {
        // TODO Auto-generated method stub
        System.out.println(str);
    }
}

//抽象主题
public interface Subject{
    public void addObserver(Observer observer);
    public void removeObserver(Observer observer);
    public void notifyObservers(String str);
}

//具体主题
public class ConcreteSubject implements Subject{
    // 存放观察者
    private List<Observer> list = new ArrayList<Observer>();

      //注册观察者
    @Override
    public void addObserver(Observer observer) {
        // TODO Auto-generated method stub
        list.add(observer);
    }

      //移除观察者
    @Override
    public void removeObserver(Observer observer) {
        // TODO Auto-generated method stub
        list.remove(observer);
    }

      // 遍历所有观察者，并通知
    @Override
    public void notifyObservers(String str) {
        // TODO Auto-generated method stub
        for(Observer observer:list){
            observer.update(str);
        }
    }
}

public class Test {
    public static void main(String[] args) {
        //一个主题
        ConcreteSubject eatSubject = new ConcreteSubject();
        //两个观察者
        ConcreteObserver p1 = new ConcreteObserver();
        ConcreteObserver p2 = new ConcreteObserver();

        //观察者订阅主题
        eatSubject.addObserver(p1);
        eatSubject.addObserver(p2);
        eatSubject.notifyOBservers("起来敲代码啦！！！");

    }
}
```

https://juejin.im/post/5bc96afff265da0aa94a4493

https://www.tutorialspoint.com/design_pattern/bridge_pattern.htm

* PULL模式
  
  而拉模型是主题对象不知道观察者具体需要什么数据，没有办法的情况下，干脆把自身传递给观察者，让观察者自己去按需要取值。

https://www.bilibili.com/video/BV1W4411c77E?p=120