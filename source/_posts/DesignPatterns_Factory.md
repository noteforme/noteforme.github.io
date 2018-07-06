---
title: DesignPatterns  Factory
comments: true
date: 2017-08-10 08:36:29
tags:
categories: DesignPatterns

---

设计模式：做到高内聚　低耦合



#  Factory（工厂模式）

## 工厂类　对实例类进行管理和创建


       public class Factory {
        //静态工厂方法
        public static Cat produceCat() {
            return new Cat();
        }
    
       public static Dog produceDog() {
            return new Dog();
        }
    
        public static void main(String[] args) {
            Animal cat = Factory.produceCat();
            cat.move();
    
            Dog dog = Factory.produceDog();
            dog.move();
            dog.eatBone();
         }
        }`
　　[code地址](https://github.com/BlogForMe/JAVA/blob/master/DesignPatterns/src/com/designparrerns/factory/Standard/Factory.java)
　　假如我要实例化一个pig对象,那么肯定是要在Factory做添加的，接下来看看可以不修改其他类实现需求

## Abstract Factory(抽象工厂)


     public interface Provider {
      Animal produce();
     }
     
     public class DogFactory implements  Provider {
       @Override
       public Animal produce() {
        return new Dog();
       }
     }

 

    public class CatFactory implements Provider {
        @Override
        public Animal produce() {
            return new Cat();
        }
    }

  

  然后调用
    Provider provider = new CatFactory();
        Animal cat = provider.produce();
        cat.move();
  `如果要实例化Pig，添加一个PigFactory 就可以了

