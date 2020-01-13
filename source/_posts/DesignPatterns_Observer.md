---
title: DesignPatterns_Observer
comments: true
date: 2018-06-06 16:09:58
tags:
categories: DesignPatterns
---



## 　Observer观察者模式/订阅模式

主题对象变化，通知所有的观察者
//抽象观察者
public interface Observer{
public void update(String str);
}

```
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
    @Override
    public void addObserver(Observer observer) {
        // TODO Auto-generated method stub
        list.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        // TODO Auto-generated method stub
        list.remove(observer);
    }

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