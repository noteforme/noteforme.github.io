---
title: DesignPatterns  Factory
comments: true
date: 2017-08-10 08:36:29
tags:
categories: DesignPatterns

---



##### 工厂方法模式

![2021-09-12_6.40.17_factory](DesignPatterns_Factory/2021-09-12_6.40.17_factory.png)



![2021-09-12_6.41.32_fac_indraduce](DesignPatterns_Factory/2021-09-12_6.41.32_fac_indraduce.png)

工厂方法和抽象工厂方法 都是通过工厂生产产品，由于产品都实现了相同的接口，根据获取到的产品作相应的处理

工厂方法 :返回一个产品实例 ,  继承了工厂Dialog Base类,Base类抽象方法由sub类实现

抽象工厂方法:可以返回一组相关的产品实例,实现了创建不同产品的抽象工厂接口





##### Simple factory

```
public class PizzaStore {

    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza = factory.createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```



```
public class SimplePizzaFactory {

    public Pizza createPizza(String type) {
        Pizza pizza = null;
        if ("cheese".equals(type)) {
            pizza = new CheesePizza();
        } else if ("pepperoni".equals(type)) {
            pizza = new PepperoniPizza();
        } else if ("calm".equals(type)) {
            pizza = new GreekPizza();
        }
        return pizza;
    }
}
```



```
public class PizzaStore {

    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }


    public Pizza orderPizza(String type) {
        Pizza pizza = factory.createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```



```
public class Pizza {
    String name;
    String dough;
    String sauce;
    ArrayList topping = new ArrayList();

   public   void prepare() {
        printOperation("preparing     " + name);
        System.out.println("Tossing dough...");
        System.out.println("Adding sauce");
        System.out.println("Adding topping: ");
        for (int i=0;i<topping.size();i++){
            System.out.println("    "+topping.get(i));
        }
    }

    public  void bake() {
        printOperation("bake  for 25 minutes at 350");
    }

    public   void cut() {
        printOperation("Cutting the pizza into diagonal slices");
    }

    public  void box() {
        printOperation("Place pizza in official pizzaStore bx");
    }

    public  void printOperation(String op) {
        System.out.println(/*getClass().getSimpleName() + " " +*/ op);
    }

    public String getName(){
        return  name;
    }

}
```



```
SimplePizzaFactory calmFactory = new SimplePizzaFactory();
PizzaStore calmPizza = new PizzaStore(calmFactory);
calmPizza.orderPizza("calm");
```





##### Static Factory（静态工厂模式）

```
public interface Animal {
    void move();
}
```



```
public class Cat implements Animal{
    public void move() {
        System.out.println("Cat move");
    }
}
```



1. 工厂类　对实例类进行管理和创建


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

假如我要实例化一个pig对象,那么肯定是要在Factory做添加的，接下来看看可以不修改其他类实现需求

2. Abstract Factory(抽象工厂 elementary )


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

​    Provider provider = new CatFactory();
​     Animal cat = provider.produce();
​    cat.move();
  如果要实例化Pig，添加一个PigFactory 就可以了

##### 不同Button实例

```
/**
 * // 抽象工厂接口声明了一组能返回不同抽象产品的方法。这些产品属于同一个系列
 * // 且在高层主题或概念上具有相关性。同系列的产品通常能相互搭配使用。系列产
 * // 品可有多个变体，但不同变体的产品不能搭配使用。
 */
public interface GUIFactory {
    Button createButton();
    CheckBox createCheckBox();
}
```

```
// Concrete factories produce a family of products that belong
// to a single variant. The factory guarantees that the
// resulting products are compatible. Signatures of the concrete
// factory's methods return an abstract product, while inside
// the method a concrete product is instantiated.
public class WinFactory implements  GUIFactory {
    public Button createButton() {
        return new WinButton();
    }

    public CheckBox createCheckBox() {
        return new WinCheckbox();
    }
}
```

```
public class MacFactory implements  GUIFactory {
    public Button createButton() {
        return new MacButton();
    }

    public CheckBox createCheckBox() {
        return new MacCheckbox();
    }
}
```

```
/**
 * // 系列产品中的特定产品必须有一个基础接口。所有产品变体都必须实现这个接口。
 */
public interface Button {
    void  paint();
}
```

```
// Concrete products are created by corresponding concrete
// factories.
public class WinButton implements Button{
    public void paint() {
        System.out.println("根据 Windows 样式渲染按钮   WinButton");
    }
}
```

```
public class MacButton implements Button{
    public void paint() {
        System.out.println("根据 macOs 样式渲染按钮 MacButton");
    }
}
```

```
// 这是另一个产品的基础接口。所有产品都可以互动，但是只有相同具体变体的产
// 品之间才能够正确地进行交互。
public interface CheckBox {
    void  paint();
}
```

```
public class WinCheckbox implements  CheckBox {
    public void paint() {
        System.out.println("根据 Win 样式渲染复选框。   WinCheckbox");
    }
}
```

```
public class MacCheckbox implements  CheckBox {
    public void paint() {
        System.out.println("根据 macOS 样式渲染复选框。   MacCheckbox");
    }
}
```

```
public class Application {
    private GUIFactory factory;
    private Button button;
    private CheckBox checkBox;
    public Application(GUIFactory factory) {
        this.factory = factory;
    }

    public void createUi() {
        this.button = factory.createButton();
        this.checkBox = factory.createCheckBox();
    }

    public  void paint(){
        button.paint();
        checkBox.paint();
    }
}
```



https://refactoring.guru/design-patterns/abstract-factory

https://www.zhihu.com/question/20367734

##### 不同Dialog实例

```
public abstract class Dialog {
    // The creator may also provide some default implementation
    // of the factory method.
    abstract Button createButton();

    // Note that, despite its name, the creator's primary
    // responsibility isn't creating products. It usually
    // contains some core business logic that relies on product
    // objects returned by the factory method. Subclasses can
    // indirectly change that business logic by overriding the
    // factory method and returning a different type of product
    // from it.
    void render() {
        // Call the factory method to create a product object.
        Button okButton = createButton();
        // Now use the product.
        okButton.onClick("closeDialog");
        okButton.render();
    }
}
```



```
public class WindowsDialogFac extends Dialog {
    Button createButton() {
        return new WindowsButton();
    }
}
```

```
public class WebDialogFac extends Dialog {
    Button createButton() {
        return new HTMLButton();
    }
}
```

```
public interface Button {
    void render();
    void onClick(String  cls);
}
```

```
public class WindowsButton implements Button {
    public void render() {
        System.out.println("WindowsButton   render()");
    }

    public void onClick(String cls) {
        System.out.println("WindowsButton   onClick "+cls);
    }
}
```

```
public class HTMLButton implements Button {
    public void render() {
        System.out.println("HTMLButton   render()");
    }

    public void onClick(String cls) {
        System.out.println("HTMLButton   onClick "+cls);
    }
}
```

```
public class Application {
    Dialog dialog;

    String configOs = "Windows";


    void initialize() throws Exception {
        if (configOs.equals("Windows")) {
            dialog = new WindowsDialogFac();
        } else if (configOs.equals("HTML")) {
            dialog = new WebDialogFac();
        } else
            throw new Exception("Error! Unknown operating system.");
    }

    // The client code works with an instance of a concrete
    // creator, albeit through its base interface. As long as
    // the client keeps working with the creator via the base
    // interface, you can pass it any creator's subclass.
    public void main() throws Exception {
        initialize();
        dialog.render();
    }

}
```





##### 抽象工厂

简单工厂的工厂

