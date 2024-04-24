---
title: DesignPatterns  Factory
comments: true
date: 2017-08-10 08:36:29
tags:
categories: DesignPatterns

---



工厂方法模式   ： 只创建一种类型的实例，
抽象工厂方法模式 ： 创建多种类型的实例, 抽象工厂 只有一个实例后，也就是工厂方法。

https://www.youtube.com/watch?v=EdFq_JIThqM

# origin

1. donnot have a common base class

```
public class Restuarant{
    public ???? orderBuger(String request){
        if("BEEF".equals(request)){
            BeefBurger burger  = new BeefBurger();
            burget.prepare();
            return burger;
        }else if("VEGGIE".equals(request)){
            VeggieBurger burger = new VeggieBurger();
            burget.prepare();
            return burger;
        }
    }
}

```





![2021-09-12_6.41.32_fac_indraduce](DesignPatterns_Factory/2021-09-12_fac_indraduce.png)

工厂方法和抽象工厂方法 都是通过工厂生产产品，由于产品都实现了相同的接口，根据获取到的产品作相应的处理

工厂方法 :返回一个产品实例 ,  继承了工厂Dialog Base类,Base类抽象方法由sub类实现

抽象工厂方法:可以返回一组相关的产品实例,实现了创建不同产品的抽象工厂接口

# Simple factory


is not a full-fledged offical pattern

UML

```mermaid

classDiagram
	class Restaurant{
		+ Burger orderBurger()
	}
	
	class SimpleBurgetFactory{
		+ Burger createBurget()
	}

  class Burger {
  	+ int productId  
    + String addOns
    + prepare()
  }
 
  class BeefBurger {
  	+ boolean angus
    + prepare()
  }
  
  class VeggieBurger {
  	+ boolean combo
    + prepare()
  }

Burger  <|--  BeefBurger
Burger  <|--  VeggieBurger
Burger  <.. SimpleBurgetFactory
SimpleBurgetFactory <.. Restaurant

```



implements

   
```
public class SimpleBurgetFactory{
    public Burger orderBuger(String request){
        if("BEEF".equals(request)){
            BeefBurger burger  = new BeefBurger();
        }else if("VEGGIE".equals(request)){
            VeggieBurger burger = new VeggieBurger();
        }
        burget.prepare();
        return burger;
    }
}
```



#  Factory method 

如果我们想新增一种类型或者修改，一种类型，那么就需要修改上面简单工厂的代码。

## Standard Uml

标准工厂UML


```mermaid
classDiagram
  class Creator {
  	+ someOperation()
    + factoryMethod(): Product
  }

  class ConcreteCreatorA {
    +createProduct(): Product
  }

  class ConcreteCreatorB {
    +createProduct(): Product
  }

  class Product {
    <<interface>> 
    +operation(): void
  }

  class ConcreteProductA {
  }

  class ConcreteProductB {
  }

  Creator ..> Product : Dependency

  Creator <|-- ConcreteProductA : Inheritance
  Creator <|-- ConcreteProductB : Inheritance
  Product <|.. ConcreteCreatorA : Realization
  Product <|.. ConcreteCreatorB : Realization

```

我们的工厂不再创造对象，而是把这个构建过程移到子类。



UML

```mermaid
classDiagram
  class Restaurant {
    + someOperation()
    + factoryMethod(): Burger
  }

  class BeefBurgetRestaurant {
    +createProduct(): Burger
  }

  class VeggieBurgetRestaurant {
    +createProduct(): Burger
  }

  class Burger {
    <<interface>> 
    +prepare(): void
  }

  class BeefBurger {
  }

  class VeggieBurger {
  }

  Restaurant ..> Burger : Dependency
  Restaurant <|-- BeefBurgetRestaurant : Inheritance
  Restaurant <|-- VeggieBurgetRestaurant : Inheritance
  Burger <|.. BeefBurger : Realization
  Burger <|.. VeggieBurger : Realization
```




```
public class Restaurant{
    public Burger orderBuger(String request){
        Burger burger  = createBurget();
        burget.prepare();
        return burger;
    }
    public abstract Burget createBurget();
}

public class BeefBurgetRestaurant extends Restaurant{
    @Override
    public  Burget createBurget(){
        return new BeefBurger();
    }
}

public class VeggieBurgetRestaurant extends Restaurant{
    @Override
    public  Burget createBurget(){
        return new  VeggieBurger();
    }
}

public interface Burger{
    void prepare();
}

public class BeefBurger implements Burger{
    void prepare(){}
}

public class VeggieBurger implements Burger{
    void prepare(){}
}

/** 原来的代码没有， 可以尝试这种方式更好*/
public class Store{
    privste Restaurant factory;
    public Store(Restaurant factory){
    	this.factory = factory;
    }
   public orderBuger(String request){
	factory.orderBuger(request)
   } 
}


public static void main(String[] args) {
    BeefBurgetRestaurant beefResto = new BeefBurgetRestaurant();
    Burger beefBurger =  beefResto.orderBurget();

    VeggieBurgetRestaurant veggieResto = new VeggieBurgetRestaurant();
    Burger beefBurger =  veggieResto.orderBurget();
}

```


如果我们需要新开另一家店 ITALIAN 餐厅,那么实现方式，又需要在工厂里 判断,创建另一个ITALIAN工厂,这样又比较耦合了。

```
public class BeefBurgetRestaurant extends Restaurant{
    @Override
    public  Burget createBurget(String request){
         Burger burger = null;
         if("ITALIAN".equals(request)){
             burger  = new ItalianBeefBurger();
         }else if("VEGGIE".equals(request)){
             burger = new AmericanBeefBurger();
         }	
        return burger;
    }
}

public class VeggieBurgetRestaurant extends Restaurant{
    @Override
    public  Burget createBurget(String request){
         Burger burger = null;
         if("ITALIAN".equals(request)){
             burger  = new ItalianVeggieBurger();
         }else if("VEGGIE".equals(request)){
             burger = new AmericanVeggieBurger();
         }	
        return burger;
    }
}

```



# Abstract Factory(抽象工厂 elementary )

在原有生产GPU的产品线上，好需要添加监控的功能，此时就会出现如下 代码耦合的情况。

<img width="1408" alt="Screenshot 2024-04-24 at 16 49 43" src="https://github.com/noteforme/noteforme.github.io/assets/6995071/b55e30a3-fd6e-43db-b678-e7db21251628">


```mermaid
classDiagram
  class AbstractFactory {
    +createProductA(): AbstractProductA
    +createProductB(): AbstractProductB
  }

  class ConcreteFactory1 {
    +createProductA(): AbstractProductA
    +createProductB(): AbstractProductB
  }

  class ConcreteFactory2 {
    +createProductA(): AbstractProductA
    +createProductB(): AbstractProductB
  }

  class AbstractProductA {
    +operationA(): void
  }

  class ConcreteProductA1 {
    +operationA(): void
  }

  class ConcreteProductA2 {
    +operationA(): void
  }

  class AbstractProductB {
    +operationB(): void
  }

  class ConcreteProductB1 {
    +operationB(): void
  }

  class ConcreteProductB2 {
    +operationB(): void
  }

  AbstractFactory <|-- ConcreteFactory1
  AbstractFactory <|-- ConcreteFactory2
  AbstractFactory o-- AbstractProductA
  AbstractFactory o-- AbstractProductB
  ConcreteFactory1 o-- ConcreteProductA1
  ConcreteFactory1 o-- ConcreteProductB1
  ConcreteFactory2 o-- ConcreteProductA2
  ConcreteFactory2 o-- ConcreteProductB2
```


implements
```

public abstract class Company {
    public abstract Gpu createGpu();
    public abstract Monitor createMonitor();
}

public class AsusManufacturer extends Company {
    @Override
    public Gpu createGpu() {
        return new AsusGpu();
    }
    @Override
    public Monitor createMonitor() {
        return new AsusMonitor();
    }
}

public class MsiManufacturer extends Company {

    @Override
    public Gpu createGpu() {
        return new MsiGpu();
    }

    @Override
    public Monitor createMonitor() {
        return new MsiMonitor();
    }

}

    public static void main(String[] args) {

        Company msi = new MsiManufacturer();
        Company asus = new AsusManufacturer();

        List.of(msi.createGpu(), msi.createMonitor(), asus.createGpu(), asus.createMonitor())
                .forEach(Product::assemble);

    }



```




https://blog.csdn.net/qq_18242391/article/details/81503370
https://github.com/geekific-official/geekific-youtube/blob/main/design-patterns/creational-abstract-factory/src/main/java/com/youtube/geekific/MainApp.java

