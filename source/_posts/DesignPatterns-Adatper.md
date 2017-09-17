---
title: DesignPatterns-Adatper
comments: true
date: 2017-08-10 15:41:04
tags:
categories: DesignPatterns

---

## 　适配器模式


     //需要适配的类
    public class Adaptee {
      public  void specificRequest(){
        System.out.println("需要适配的类");
      }
    }
    
     //目标接口
    public interface Target {
     void request();
    }
    
#  　对象适配器（采用对象组合方式实现）

    public class Adapter implements Target {
        //直接关联被适配类
        private Adaptee adaptee;

        //可以通过构造函数传入具体需要适配的被适配类对象
        public Adapter(Adaptee adaptee) {
            this.adaptee = adaptee;
        }

        @Override
        public void request() {
            this.adaptee.specificRequest();
        }

        public static void main(String[] args) {
            Target adapter = new Adapter(new Adaptee());
            adapter.request();
        }
    }

# 　类适配器模式(采用继承实现)

    public class CAdapter extends Adaptee implements Target {
        @Override
        public void request() {
            super.specificRequest();
        }

        public static void main(String[] args) {

            Target adapter = new CAdapter();
            adapter.request();
        }
    }

##    　Observer观察者模式/订阅模式

  主题对象变化，通知所有的观察者
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

##　Builder模式

参考　：https://github.com/helen-x/AndroidInterview/blob/master/android/Android%20%E6%BA%90%E7%A0%81%E4%B8%AD%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F(%E4%BD%A0%E9%9C%80%E8%A6%81%E7%9F%A5%E9%81%93%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E5%85%A8%E5%9C%A8%E8%BF%99%E9%87%8C).md