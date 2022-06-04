---
title: DesignPattern_Proxy
comments: true
date: 2021-01-14 11:41:21
tags: 
categories: DesignPatterns
---



#### 静态代理

##### 静态代理实现

编译的时候就已经存在

![](https://www.oodesign.com/images/design_patterns/structural/proxy-design-pattern-implementation-uml-class-diagram.png)



代理模式

1. 代理类与委托类有同样的接口
2. 代理类主要负责为委托类预处理消息、过滤消息、把消息转发给委托类，以及事后处理消息等。
3. 一个代理类的对象与一个委托类的对象关联，代理类的对象本身并不真正实现服务，而是通过调用委托类的对象的相关方法，来提供特定的服务。



##### kotlin 委托的作用

```kotlin
interface Base {
    fun print()
}

class BaseImpl(val x: Int) : Base {
    override fun print() { print(x) }
}

class Derived(b: Base) : Base by b   // kotlin这里自动生成了java代理中 ZhangYuXinProxy

fun main() {
    val b = BaseImpl(10)
    Derived(b).print()
}
```



##### java代理



```java
public interface ZyxInterface {
    void sayHello();
}

public class ZhangYuXin implements  ZyxInterface {
    @Override
    public void sayHello() {
        System.out.println("Hello from zyx");
    }
}
```



代理类

```java
public class ZhangYuXinProxy implements ZyxInterface {
    private ZhangYuXin zhangYuXin;

    public ZhangYuXinProxy(ZhangYuXin zhangYuXin) {
        this.zhangYuXin = zhangYuXin;
    }

    @Override
    public void sayHello() {
        System.out.println("拍电影前的准备工作");
        zhangYuXin.sayHello();
        System.out.println("拍电影后的收尾工作");
    }
}
```



```java
ZhangYuXin zyxHello = new ZhangYuXin();
ZhangYuXinProxy zhangYuXin = new ZhangYuXinProxy(zyxHello);
zhangYuXin.sayHello();
```

运行结果

> 拍电影前的准备工作
> Hello from zyx
> 拍电影后的收尾工作



#### 动态代理

 通过反射机制生成的代理对象

![](https://static.packt-cdn.com/products/9781785888038/graphics/B05180_05_01.jpg)





##### 动态代理原理 

生成的动态代理Class文件

```java
byte[] classFile = ProxyGenerator.generateProxyClass("BinInterface$0", new Class[]{ZyxInterface.class});
try {
    FileOutputStream out = new FileOutputStream( "BinInterface$0.class");
    out.write(classFile);
    out.flush();
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```

BinInterface$0.class

```java
public final class BinInterface$0 extends Proxy implements ZyxInterface {
    private static Method m1;
    private static Method m3;
    private static Method m2;
    private static Method m0;

    public BinInterface$0(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

  	//调用接口的方法就会调用sayHello方法
    public final void sayHello() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null); //h就是create方法传进来的InvocationHandler,然后回调invoke方法
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m3 = Class.forName("pattern.proxy.xiangxue.ZyxInterface").getMethod("sayHello");
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```



##### 动态代理实现

```java
public class DoSomeThingDynamic {
    Object object;

    public DoSomeThingDynamic(Object object) {
        this.object = object;
    }

    /**
     *
     * @param say 需要代理执行的接口类
     * @param <T>
     * @return 动态代理 运行时生成的一个say对应类型的类，用于调用say接口的时候 运行
     */
    public <T> T create(final Class<T> say){
        return (T) Proxy.newProxyInstance(say.getClassLoader(),
                new Class<?>[]{say},
                new InvocationHandler() {
                    //method这个函数是 反射的函数， method具体代表的是 代理在执行的当前的函数
                    @Override
                    public Object invoke(Object o, Method method, Object[] args) throws Throwable {
                        System.out.println("拍电影前的准备工作");
                        Object result = method.invoke(object, args); //等价于 zhangYuXin.sayHello();
                        System.out.println("拍电影后的收尾工作");
                        return result;
                    }
                }
        );
    }
}
```



```java
DoSomeThingDynamic say1 = new DoSomeThingDynamic(zyxHello); //动态生成代理类
ZyxInterface zyxProxy = say1.create(ZyxInterface.class);
zyxProxy.sayHello();

System.out.println("---------------------\n");

BinBin binBye = new BinBin();
DoSomeThingDynamic say2 = new DoSomeThingDynamic(binBye);	//动态生成代理类
BinInterface bbProxy = say2.create(BinInterface.class);
bbProxy.sayBye();
```

运行结果

> 拍电影前的准备工作
> Hello from zyx
>
> 拍电影后的收尾工作
>
> -----
>
> 拍电影前的准备工作
> Bye Bye from BinBin
> 拍电影后的收尾工作



https://www.bilibili.com/video/BV1ib4y1f7S1?p=10&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1uQ4y1Z7gA?p=23



https://juejin.cn/post/6844903520919879694

https://juejin.cn/post/6844903978342301709

https://time.geekbang.org/column/article/201823



