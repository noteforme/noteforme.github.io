---
title: String
date: 2017-07-21 14:54:53
tags:
categories: JAVA

---

 　字符串连接有两种方式，一种时  " +  ",另一种是 StringBuilder方式

命令行运行java

平常用开发工具习惯了，都忘了怎么用命令行运行，特此记录下

新建 Concatenation.java文件，cmd到该目录下，写入代码

     public class Concatenation{
        public static void main(String[] args){
         String mango = "mango";
         String s = "abc" + mango + "def" + 47;
         System.out.print(s);	
        }
     }

生成 Concatenation.calss 文件

      D:\DemoExo>javac Concatenation.java

运行Concatenation.class文件,后面是运行结果

       D:\DemoExo>java Concatenation
       abcmangodef47

#### String字符串连接方式

回到主题

  查看JDK文档（不知道在哪），String 类中每一个看起来会修改String值的方法，实际上都是创建了一个全新的String对象,以包含修改后的字符串内容.
       
    public class Immutable{
         public static String upcase(String s){
           return s.toUpperCase();
         }
         public static void println(String q){
           System.out.println(q);
         }
     
        public static void main(String[] args){
            String q = "howdy";
            print(q);            // howdy
    	   String qq= upcase(q);
           print(qq);            //HOWDY
           print(q);             //howdy
        }
    }
运行结果：
howdy
HOWDY
howdy
当把q传给upcase()时，实际传递的是引用的拷贝

#### 编译器运行过程

反编译Concatenation,生成JVM字节码
       
       javap -c Concatenation.class
字节码是这样的

    C:\DemoExo>javap -c Concatenation.class
     Compiled from "Concatenation.java"
    public class Concatenation {
      public Concatenation();
        Code:
           0: aload_0
           1: invokespecial #1                  // Method java/lang/Object."<init>":()V
           4: return
    
      public static void main(java.lang.String[]);
        Code:
           0: ldc           #2                  // String mango
           2: astore_1
           3: new           #3                  // class java/lang/StringBuilder
           6: dup
           7: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
          10: ldc           #5                  // String abc
          12: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          15: aload_1
          16: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          19: ldc           #7                  // String def
          21: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          24: bipush        47
          26: invokevirtual #8                  // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
          29: invokevirtual #9                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
          32: astore_2
          33: getstatic     #10                 // Field java/lang/System.out:Ljava/io/PrintStream;
          36: aload_2
          37: invokevirtual #11                 // Method java/io/PrintStream.print:(Ljava/lang/String;)V
          40: return
    }
可以看到，编译器创建了一个StringBuilder对象,用以构造最终的String，并四次调用了append(),最后使用命令astore_2生成 s对象。
这样来看，使用 “+” 连接符，是不会影响性能的咯，接着往下

###  “+”连接符 和 StringBuilder对比

运行javap -c WhitherStringBuilder.class 看到两个方法对象的字节码，先看前半部分implicit()

    public class WhitherStringBuilder {
      public WhitherStringBuilder();
        Code:
           0: aload_0
           1: invokespecial #1                  // Method java/lang/Object."<init>":()V
           4: return
    
      public java.lang.String implicit(java.lang.String[]);
        Code:
           0: ldc           #2                  // String
           2: astore_2
           3: iconst_0
           4: istore_3
           5: iload_3
           6: aload_1
           7: arraylength
           8: if_icmpge     38
          11: new           #3                  // class java/lang/StringBuilder
          14: dup
          15: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
          18: aload_2
          19: invokevirtual #5                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          22: aload_1
          23: iload_3
          24: aaload
          25: invokevirtual #5                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
          28: invokevirtual #6                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
          31: astore_2
          32: iinc          3, 1
          35: goto          5
          38: aload_2
          39: areturn

第8到35行，在for循环体内，每一次循环就创建了一个StringBuilder对象，接着看看explicit()

     public java.lang.String explicit(java.lang.String[]);
       Code:
          0: new           #3                  // class java/lang/StringBuilder
          3: dup
          4: invokespecial #4                  // Method java/lang/StringBuilder."<init>":()V
          7: astore_2
          8: iconst_0
          9: istore_3
         10: iload_3
         11: aload_1
         12: arraylength
         13: if_icmpge     30
         16: aload_2
         17: aload_1
         18: iload_3
         19: aaload
         20: invokevirtual #5                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
         23: pop
         24: iinc          3, 1
         27: goto          10
         30: aload_2
         31: invokevirtual #6                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;

第13到27行在for循环体内，StringBuilder只会生成一个对象

总结： 由此来看，如果字符串操作比较简单，就可以用“+”连接让编译器处理字符串
如果在循环中，那么自己创建一个StringBuilder对象

参考：*摘自ThinkInJava4   P286*

####  避免创建不必要的对象

创建　String对象

 最好能重用对象而不是每次需要的时候就创建一个功能相同的新对象，重用方式即快速
又流行。如果对象是不可变的，它始终可以被重用。

极端的反面例子　｀Ｓtring s = new String("stringette");  //DON'T DO THIS ! ｀
该语句每次被执行的时候都创建一个新的String实例，传递给String构造器的参数"stringette'本身就是一个String实例，如果这种用法在一个循环中，或者在一个频繁
调用的方法中，就会创建出成千上万不必要的String  实例。

改进后的版本如下所示

```
String s  = "stringette';
```
这个版本只用了一个String实例，而且它可以保证，对于所有的同一台虚拟机中运行的代码
，只要包含相同的字符串字面常量，该对象就会被重用.

##　返回数据处理

对于同时提供静态工厂方法和构造器的不可变类，通常可以使用静态工厂方法，而不是构造器
以避免创建不必要的对象，例如，静态工厂方法Boolean.valueof(String)由于构造器Boolean(String),构造器每次调用的时候都会创建一个新的对象，而静态工厂方法则不会

我们平常解析回来后非String类型的数据，需要转换成String对象　
看到这里后就需要使用`String.valueof(String)` 


```
  /**
     * Returns the string representation of the {@code Object} argument.
     *
     * @param   obj   an {@code Object}.
     * @return  if the argument is {@code null}, then a string equal to
     *          {@code "null"}; otherwise, the value of
     *          {@code obj.toString()} is returned.
     * @see     java.lang.Object#toString()
     */
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }
```
从源码中看到对形参　做了　null判断，所以用这种方式页不用担心闪退的情况了
参考 Effective Java　page17





* java 8 获取对象数据

  ```
  private WchatMobile decodeMobile(String encryptedData, String iv, String unionid) {
    WchatMiniAppSession miniAppSession = getWchatMiniAppSession(unionid);
    String sessionKey = Optional.ofNullable(miniAppSession)
      .orElseThrow(() -> new ServiceExecutionException("用户小程序登录信息为空"))
      .getSessionKey();
    
    return Optional.ofNullable(wchatMiniAppService)
     .map(as -> as.getWxMaService())
     .map(s -> s.getUserService())
     .map(se -> se.getPhoneNoInfo(sessionKey, encryptedData, iv))
     .map(num -> WchatMobile.from(num))
     .orElse(null);
   }
  ```

  

https://time.geekbang.org/column/article/209343

#### 亨元模式

    Integer a = 1;
    Integer b = 2;
    Integer c = 3;
    Integer d = 3;
    Integer e = 321;
    Integer f = 321;
    Long g = 3L;
    System.out.println(c==d);
    System.out.println(e==f);
    System.out.println(c==(a+b));
    System.out.println(c.equals(a+b));
    System.out.println(g ==(a+b));
    System.out.println(g.equals(a+b));
