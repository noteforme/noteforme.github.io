---
title: String 字符串连接
date: 2017-07-21 14:54:53
tags:
---
# 一、命令行运行java
平常用开发工具习惯了，都忘了怎么用命令行运行，特此记录下
1、新建 Concatenation.java文件，cmd到该目录下，写入代码
     
     public class Concatenation{
        public static void main(String[] args){
	     String mango = "mango";
         String s = "abc" + mango + "def" + 47;
         System.out.print(s);	
        }
     }
 2、生成 Concatenation.calss 文件

      D:\DemoExo>javac Concatenation.java

3、运行Concatenation.class文件,后面是运行结果

       D:\DemoExo>java Concatenation
       abcmangodef47
#二、String字符串连接方式
1、回到主题：查看JDK文档（不知道在哪），String 类中每一个看起来会修改String值的方法，实际上都是创建了一个全新的String对象,以包含修改后的字符串内容.
       
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

2、编译器运行过程
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

3、“+”连接符 和 StringBuilder对比
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

参考：摘自ThinkInJava4  P286
