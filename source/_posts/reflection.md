---
id: 295
title: reflection
date: '2024-05-31T07:56:18+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=295'
permalink: /2024/05/31/reflection/
categories:
    - JAVA
---

https://jonblog.site/2024/05/28/designpattern-proxy/
https://jonblog.site/2024/06/06/annotation/

https://www.oracle.com/technical-resources/articles/java/javareflection.html
https://www.bilibili.com/video/BV1tY411Z799

# reflect demo

常用反射用法

```java
/**
 * @author xiaoyaomeng
 */
class Person {
    private int age;
    private String name;

    public Person() {
    }

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

class SuperMan extends Person implements ActionInterface {
    private boolean BlueBriefs;

    public void fly() {
        System.out.println("超人会飞耶～～");
    }

    public boolean isBlueBriefs() {
        return BlueBriefs;
    }

    public void setBlueBriefs(boolean blueBriefs) {
        BlueBriefs = blueBriefs;
    }

    @Override
    public void walk(int m) {
        // TODO Auto-generated method stub
        System.out.println("超人会走耶～～走了" + m + "米就走不动了！");
    }
}

interface ActionInterface {
    public void walk(int m);
}




public class ReflectMethod {

    /**
     * Demo1: 通过Java反射机制得到类的包名和类名
     */
    public static void Demo1() {
        Person person = new Person();
        System.out.println("Demo1: 包名: " + person.getClass().getPackage().getName() + "，"
                + "完整类名: " + person.getClass().getName());
    }

    /**
     * Demo2: 验证所有的类都是Class类的实例对象
     *
     * @throws ClassNotFoundException
     */
    public static void Demo2() throws ClassNotFoundException {
        //定义两个类型都未知的Class , 设置初值为null, 看看如何给它们赋值成Person类
        Class<?> class1 = null;
        Class<?> class2 = null;

        //写法1, 可能抛出 ClassNotFoundException [多用这个写法]
        class1 = Class.forName("reflection.Person");
        System.out.println("Demo2:(写法1) 包名: " + class1.getPackage().getName() + "，"
                + "完整类名: " + class1.getName());

        //写法2
        class2 = Person.class;
        System.out.println("Demo2:(写法2) 包名: " + class2.getPackage().getName() + "，"
                + "完整类名: " + class2.getName());
    }

    /**
     * Demo3: 通过Java反射机制，用Class 创建类对象[这也就是反射存在的意义所在]
     */
    public static void Demo3() throws ClassNotFoundException, InstantiationException, IllegalAccessException {
        Class<?> class1 = null;
        class1 = Class.forName("reflection.Person");
        //由于这里不能带参数，所以你要实例化的这个类Person，一定要有无参构造函数哈～
        Person person = (Person) class1.newInstance();
        person.setAge(20);
        person.setName("LeeFeng");
        System.out.println("Demo3: " + person.getName() + " : " + person.getAge());
    }

    /**
     * Demo4: 通过Java反射机制得到一个类的构造函数，并实现创建带参实例对象
     */
 public static void Demo4() throws ClassNotFoundException, IllegalArgumentException, InstantiationException, IllegalAccessException, InvocationTargetException {

        // Get the class object
        Class<?> clazz = Person.class;

        // Get the constructors of the class
        Constructor<?>[] constructors = clazz.getConstructors(); // all constructor methods

        // Iterate over constructors and print parameter types
        for (Constructor<?> constructor : constructors) {
            System.out.println("Constructor: " + constructor);

            // Get parameter types
            Class<?>[] parameterTypes = constructor.getParameterTypes();

            for (Class<?> paramType : parameterTypes) {
                System.out.println("Parameter type: " + paramType.getName());
            }

            // Alternatively, you can use Parameter class to get more details
            Parameter[] parameters = constructor.getParameters();

            for (Parameter parameter : parameters) {
                System.out.println("Parameter: " + parameter.getName() + ", Type: " + parameter.getType());
            }
            System.out.println();
        }


        try {
            /**
             *  public Person(String name) 构造方法 Create 对象
             */
            final Class<?>[] stringParam =
                    {String.class};
            final Constructor<?> consString = Person.class.getConstructor(stringParam);
            Person personString = (Person) consString.newInstance(new Object[]{99});
            System.out.println("stringParam " + personString);


            /**
             *  public Person(int age, String name) 构造方法 Create 对象
             */
            final Class<?>[] stringIntParam =
                    {int.class, String.class};
            final Constructor<?> consStringInt = Person.class.getConstructor(stringIntParam);
            Person person1 = (Person) consStringInt.newInstance(new Object[]{99, "john"});
            System.out.println("stringIntParam " + person1);

        } catch (NoSuchMethodException e) {
            throw new RuntimeException(e);
        }


        Class<?> class1 = null;
        Person person1 = null;
        Person person2 = null;

        class1 = Class.forName("reflection.Person");
        //得到一系列构造函数集合
        Constructor<?>[] constructors1 = class1.getConstructors();

        person1 = (Person) constructors1[0].newInstance();
        person1.setAge(30);
        person1.setName("leeFeng");

        //constructor onject that have two params:  age and name
        person2 = (Person) constructors1[1].newInstance(20, "leeFeng");

        System.out.println("Demo4: " + person1.getName() + " : " + person1.getAge()
                + "  ,   " + person2.getName() + " : " + person2.getAge()
        );

    /**
     * Demo5: 通过Java反射机制操作成员变量, set 和 get
     */
    public static void Demo5() throws IllegalArgumentException, IllegalAccessException, SecurityException, NoSuchFieldException, InstantiationException, ClassNotFoundException {
        Class<?> class1 = null;
        class1 = Class.forName("reflection.Person");
        Object obj = class1.newInstance();

        Field personNameField = class1.getDeclaredField("name");
        personNameField.setAccessible(true);
        personNameField.set(obj, "胖虎先森");

        System.out.println("Demo5: 修改属性之后得到属性变量的值：" + personNameField.get(obj));
    }


    /**
     * Demo6: 通过Java反射机制得到类的一些属性： 继承的接口，父类，函数信息，成员信息，类型等
     *
     * @throws ClassNotFoundException
     */
    public static void Demo6() throws ClassNotFoundException {
        Class<?> class1 = null;
        class1 = Class.forName("reflection.SuperMan");

        //取得父类名称
        Class<?> superClass = class1.getSuperclass();
        System.out.println("Demo6:  SuperMan类的父类名: " + superClass.getName());

        System.out.println("===============================================");


        Field[] fields = class1.getDeclaredFields();
        for (int i = 0; i < fields.length; i++) {
            System.out.println("类中的成员: " + fields[i]);
        }
        System.out.println("===============================================");


        //取得类方法
        Method[] methods = class1.getDeclaredMethods();
        for (int i = 0; i < methods.length; i++) {
            System.out.println("Demo6,取得SuperMan类的方法：");
            System.out.println("函数名：" + methods[i].getName());
            System.out.println("函数返回类型：" + methods[i].getReturnType());
            System.out.println("函数访问修饰符：" + Modifier.toString(methods[i].getModifiers()));
            System.out.println("函数代码写法： " + methods[i]);
        }

        System.out.println("===============================================");

        //取得类实现的接口,因为接口类也属于Class,所以得到接口中的方法也是一样的方法得到哈
        Class<?> interfaces[] = class1.getInterfaces();
        for (int i = 0; i < interfaces.length; i++) {
            System.out.println("实现的接口类名: " + interfaces[i].getName());
        }

    }

    /**
     * Demo7: 通过Java反射机制调用类方法
     */
    public static void Demo7() throws ClassNotFoundException, SecurityException, NoSuchMethodException, IllegalArgumentException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class<?> class1 = null;
        class1 = Class.forName("reflection.SuperMan");

        System.out.println("Demo7: \n调用无参方法fly()：");
        Method method = class1.getMethod("fly");
        method.invoke(class1.newInstance());

        System.out.println("调用有参方法walk(int m)：");
        method = class1.getMethod("walk", int.class);
        method.invoke(class1.newInstance(), 100);

        method.invoke(new SuperMan(), 666); // 这个方法和上面的一样
    }

    /**
     * Demo8: 通过Java反射机制得到类加载器信息
     * <p>
     * 在java中有三种类类加载器。[这段资料网上截取]
     * <p>
     * 1）Bootstrap ClassLoader 此加载器采用c++编写，一般开发中很少见。
     * <p>
     * 2）Extension ClassLoader 用来进行扩展类的加载，一般对应的是jre\lib\ext目录中的类
     * <p>
     * 3）AppClassLoader 加载classpath指定的类，是最常用的加载器。同时也是java中默认的加载器。
     *
     * @throws ClassNotFoundException
     */
    public static void Demo8() throws ClassNotFoundException {
        Class<?> class1 = null;
        class1 = Class.forName("reflection.SuperMan");
        String nameString = class1.getClassLoader().getClass().getName();
        System.out.println("Demo8: 类加载器类名: " + nameString);
    }
}
```

# class

类是程序的一部分，每个类都有一个Class对象，每当编写并编译了一个新类就会产生一个Class对象（保存在一个同名的.class文件中）。为了生成这个对象就要用到JVM
分析主要的类和方法

```java
interface HasBatteries {}
interface Waterproof {}
interface Shoots {}
class Toy {
    //Comment out the following default constructor
    //to see NoSuchMethodError from(*1*)
    Toy() { }
    Toy(int i) { }
}

class FancyToy extends Toy implements HasBatteries, Waterproof, Shoots {
    FancyToy() {
        super(1);
    }
}

public class ToyTest {
    static void printInfo(Class cc) {
        Print.print("Class name: " + cc.getName() + "     is Interface ?  [" + cc.isInterface() + "]");  //是否是接口
        Print.print("Simple name: " + cc.getSimpleName());       //不好含包名的类名
        Print.print("Canonical name : " + cc.getCanonicalName()); //全限定的类名
    }

    public static void main(String[] args) {
        Class c = null;
        try {
            c = Class.forName("TypeInfomation.Demo.FancyToy");
        } catch (ClassNotFoundException e) {
            Print.print("Can't find FancyToy");
            System.exit(1);
        }
        Print.print(c);
        printInfo(c);
        Print.print();
        for (Class face : c.getInterfaces()) {
            printInfo(face);
            Print.print();
        }
        Class up = c.getSuperclass();
        Object obj = null;
        try {
            //Requires default constructor:
            obj = up.newInstance();
        } catch (InstantiationException e) {
            Print.print("Cannot instantiate");
            System.exit(1);
        } catch (IllegalAccessException e) {
            Print.print("Cannot access");
            System.exit(1);
        }
        printInfo(obj.getClass());
    }
}
```

# Class类

所有的Class对象都属于这个 类的一个static成员，Class对象和其他对象一样，我们可以使用  `Class.forName("Gum")`  获取并操作它的引用（这就是类加载器的工作）， 从上面的代码可以看到，getSuperclass()方法查询其直接基类，这将返回用来进一步查询的Class对象。由此，可以在运行时发现一个对象完整的类继承结构

## newInstance方法

Class的newInstance()实现“虚拟构造器”的一种途径，虚拟构造器寻你生命"我不知道你的确切类型，但是无论如何要正确地创建自己"，代码中的  up 只是一个Class引用，在编译期不具备任何更进一步的类型信息，创建新实例时，会得到指向Toy对象的Object引用，可以转型操作它。

# 动态代理

```java
class  DynamicProxyHandler implements InvocationHandler{
    private Object proxied;
    public DynamicProxyHandler(Object proxied) {
        this.proxied = proxied;
    }
  //proxy:对象的引用 ,method :被调用的方法 ，args:方法里面的参数
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {   
        System.out.println("**** proxy: " + proxy.getClass() + ". method: " +method + ", args " + args);
        if (args !=null)
            for (Object arg : args)             //方法里面的参数
                System.out.println(" " + arg);
        return method.invoke(proxied,args);  //通过方法调用 引用和参数,转发调用
    }
}

public class SimpleDynamicProxy {
   public  static  void consumer(Interface iface){
       iface.doSomething();
       iface.somethingElse("bonobo");
   }

    public static void main(String[] args) {
        RealObject real = new RealObject();
//        consumer(real);                   //常用调用
        //Insert a proxy and cal again:
        Interface proxy = (Interface) Proxy.newProxyInstance(Interface.class.getClassLoader(),
                new Class[]{Interface.class},new DynamicProxyHandler(real));
        consumer(proxy);
   }
}
```

more demo about dynamic proxy in this article 

https://jonblog.site/2024/05/28/designpattern-proxy/

2、javap
      一个随JDK发布的反编译工具
       命令行  到Cat.java所对应的Cat.class文件的位置
        运行 `javap -private Cat.class` 可以反编译源程序

3、没有任何方式可以阻止反射到达并调用那些非公共访问权限的方法

```java
public class ModifyingPrivateFields {
    public static void main(String[] args) throws Exception {
        WithPrivateFinalField pf = new WithPrivateFinalField();
        System.out.println(pf);

        Field f = pf.getClass().getDeclaredField("i");
        f.setAccessible(true);
        System.out.println("f.getInt(pf): " + f.getInt(pf));
        f.setInt(pf,47);
        System.out.println(pf);

        Print.print();
        f =pf.getClass().getDeclaredField("s");
        f.setAccessible(true);
        System.out.println("f.get(pf): "+f.get(pf));
        f.set(pf,"you are totally safe");
        System.out.println(pf);

        Print.print();
        f = pf.getClass().getDeclaredField("s2");
        f.setAccessible(true);
        System.out.println("f.get(pf): "+f.get(pf));
        f.set(pf,"No,you're not!");
        System.out.println(pf);
    }
}


class WithPrivateFinalField {
    private int i = 1;
    private final String s = " I'm totally safe";
    private String s2 = "Am I safe?";

    public String toString() {
        return "i = " + i + ", " + s + ", " + s2;
    }
}
```

https://github.com/hengzhou/Rejection
http://blog.csdn.net/ljphhj/article/details/12858767

this video write code  simulate JDK dynamic proxy

https://www.bilibili.com/video/BV1Ve411o7WM

# 反射机制

Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象都能够调用它的任意一个方法；这种动态获取的信息以及动态调用对象的方法的功能称为java的反射机制。

## 基本使用

```java
package reflection;
public class MethodReflect {
    public int addResult(int addNum) {
        return addNum;
    }
}
```

```java
//使用反射第一步:获取操作类MethodDemoFieldDemo所对应的Class对象
Class < ?>cls = Class.forName("reflection.MethodReflect");
//使用MethodDemo类的class对象生成 实例
Object obj = cls.newInstance();

//获取public int addResult(int addNum)方法
Method addMethod = cls.getMethod("addResult", new Class[] {
        int.class
});
System.out.println("修饰符: " + Modifier.toString(addMethod.getModifiers()));
System.out.println("返回值: " + addMethod.getReturnType());
System.out.println("方法名称: " + addMethod.getName());
System.out.println("参数列表: " + addMethod.getParameterTypes());
int result = (int) addMethod.invoke(obj, 2);
System.out.println("调用addResult后的运行结果:" + result);

System.out.println("--------------------------------");

//获取public String toString() 方法
Method toStringMethod = cls.getMethod("toString", new Class[] {});
System.out.println("修饰符: " + Modifier.toString(toStringMethod.getModifiers()));
System.out.println("返回值: " + toStringMethod.getReturnType());
System.out.println("方法名称: " + toStringMethod.getName());
System.out.println("参数列表: " + toStringMethod.getParameterTypes());
String msg = (String) toStringMethod.invoke(obj);
System.out.println("调用toString后的运行结果:" + msg);
```

Method类的invoke(Object obj,Object  args[])方法接收的参数必须为对象,如果参数为基本类型数据,必须转换为相应的包装类型的对象。invoke()方法的返回值总是对象,如果实际被调用的方法的返回类型是基本类型数据,那么invoke()方法会把它转换为相应的包装类型的对象,再将其返回.

```java
public class InvokeTester {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public InvokeTester() {
    }

    public int add(int param1, int param2) {
        return param1 + param2;
    }

    public String echo(String mesg) {
        return "echo" + mesg;
    }

    public static void main(String[] args) throws Exception {
        Class classType = InvokeTester.class;
        Object invokertester = classType.newInstance(); //1
        Method addMethod = classType.getMethod("add", new Class[]{ //2
                int.class,
                int.class
        });

        Object result = addMethod.invoke(invokertester, new Object[]{ //3
                new Integer(100),
                new Integer(200)
        });

        System.out.println(result);
        Method echo = classType.getMethod("echo", new Class[]{
                String.class
        });
        Object obj = echo.invoke(invokertester, new Object[]{
                new String("Jon is very good!!!")
        });
        System.out.println(obj.toString());

        InvokeTester test = new InvokeTester(); //1
        test.setName("黄翊"); //2
        //Method[] methods;
        Method[] methods = test.getClass().getDeclaredMethods(); //3
        //循环查找获取id方法，并执行查看是否有返回值
        for (int i = 0; i < methods.length; i++) {
            //如果此方法有get和Id关键字则执行
            if (methods[i].getName().indexOf("get") != -1 && methods[i].getName().indexOf("Name") != -1) {
                // 获取此get方法返回值,判断是否有值,如果没有值说明即将执行的操作新增
                if (methods[i].invoke(test, null) == null) { //4
                    System.out.println("此对象没有值！！！");
                } else {
                    Object strName = methods[i].invoke(test, null);
                    System.out.println(strName);
                }
            }
        }
    }
}
```

## invoke方法的使用

实际上invoke方法的使用，和我们常见的有所区别。

我们经常创建一个对象A，A对象里面的方法getA()方法，然后A.getA()

我们采用新的方式调用
（1）弄一个方法的“替身”（其实就是构建一个Method对象，让这个Method对象来代替你现在要用的方法）
（2）然后给替身需要的对象和参数，让替身去替你调用（像替身替你去战斗）

```java
package reflection;

import org.junit.jupiter.api.Test;

import java.lang.reflect.Method;

public class InvokeTest {
    public void test(String[] arg){
        for (String string : arg) {
            System.out.println("zp is " + string);
        }
    }
    @Test
    public void invokeDemo() throws Exception {
        //获取字节码对象,这里要填好你对应对象的包的路径
        Class<InvokeTest> clazz = (Class<InvokeTest>) Class.forName("reflection.InvokeTest");
        //形式一：获取一个对象
//        Constructor con =  clazz.getConstructor();
//        InvokeTest m = (InvokeTest) con.newInstance();
        //形式二：直接new对象，实际上不是框架的话，自己写代码直接指定某个对象创建并调用也可以
        InvokeTest m = new InvokeTest();
        String[] s = new String[]{"handsome","smart"};
        //获取Method对象
        Method method = clazz.getMethod("test", String[].class); //invoke方法要比别的方法多做一步
        //调用invoke方法来调用
        method.invoke(m, (Object) s);
    }
}
```

输出结果

> zp is handsome
> zp is smart

所以使用invoke方法要比别的方法多做一步，就是构建一个Method对象，这个对象替代的是现在程序要调用方法的替代品。

而且除了参数以外，invoke还会多要一个对象，因为方法调用需要对象，所以invoke要想调用的目标方法，就需要目标方法的需要的对象。

看起来invoke方法不仅比平常方法直接调用要麻烦很多，但是你有想过吗，我只需要输入参数，我可以调用替代各种方法，在未知的情况下，根据条件决定去调用什么对象，什么方法，一下子就让代码变得灵活，这不仅是invoke的妙处，也是整个反射的妙处，在程序运行时根据条件灵活使用。

https://zhuanlan.zhihu.com/p/350058223

## Method信息

我们已经能通过`Class`实例获取所有`Field`对象，同样的，可以通过`Class`实例获取所有`Method`信息。`Class`类提供了以下几个方法来获取`Method`：

- `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
- `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
- `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
- `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

```java
class Student extends Person {
    public int getScore(String type) {
        return 99;
    }

    private int getGrade(int year) {
        return 1;
    }

//    @Override
//    public void hello() {
//        System.out.println("Student:hello");
//    }
}

class Person {
    public String getName() {
        return "Person";
    }

    public void hello() {
        System.out.println("Person:hello");
    }
}
```

```java
Class<Student> methodClass = Student.class;
Arrays.stream(methodClass.getMethods()).forEach(method -> System.out.println("method方法:    " + method));
```

打印出

> method方法:    public int reflection.liao.Student.getScore(java.lang.String)
> method方法:    public void reflection.liao.Person.hello()
> method方法:    public java.lang.String reflection.liao.Person.getName()

```java
Class<Person> methodClass = Person.class;
Arrays.stream(methodClass.getMethods()).forEach(method -> System.out.println("method方法:    " + method));
```

打印出

> method方法:    public void reflection.liao.Person.hello()
> method方法:    public java.lang.String reflection.liao.Person.getName()
> 
> ...object方法

1. getMethods()可以 获取所有`public`的`Method`（包括父类），

2. 不能获取到子类的方法,除非是是多肽实现

## getDeclaredMethod获取的Method能否调用子类?

父类class通过getDeclaredMethod获取的Method能否调用子类的对象

验证是不可以

```java
Class<?> clz = Class.forName("reflection.liao.Person");
Object o = clz.newInstance();
Method methodGetName = clz.getMethod("getName");
Object noParams = methodGetName.invoke(o);
System.out.println(noParams);

Method methodHello  = clz.getMethod("hello");
System.out.println(methodHello.invoke(o));

Method methodGetGrade = clz.getMethod("getGrade");
System.out.println(methodGetGrade.invoke(o));
```

> A Person
> Person:hello
> null        // 这个null是哪里来的

提示

java.lang.NoSuchMethodException: reflection.liao.Person.getGrade()

所以是调用不了的。

https://www.liaoxuefeng.com/wiki/1252599548343744/1264803678201760

http://www.51gjie.com/java/796.html

https://www.cnblogs.com/onlywujun/p/3519037.html>