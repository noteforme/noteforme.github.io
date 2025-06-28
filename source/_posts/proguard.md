---
title: proguard
comments: true
date: 2017-12-19 07:29:23
tags: proguard
categories: ANDROID
---

# Proguard VS R8

**Note:** R8 doesn't let you disable or enable specific optimizations, or modify the behavior of an optimization. In fact, R8 ignores any ProGuard rules that attempt to modify default optimizations, such as `-optimizations` and `-optimizationpasses`.

https://developer.android.com/topic/performance/app-optimization/add-keep-rules#kts

# ProGuard File

开启ProGuard，Android工程被Build后，会生成以下文件：<project_root>\app\build\outputs\mapping\release 

- dump.txt 
  apk文件中所有类的构成一览 

- mapping.txt 
  记录了混淆后的名字与混淆前的名字的对应关系，每一次混淆结果和映射关系都不一样。 
  当遇到Bug是，查看到的堆信息，要和混淆前的源码关联起来，所以管理这个文件很重要。 

- seeds.txt  
  未被混淆的类和方法一览 

- usage.txt 
  记录了从apk文件中删掉的代码。这个文件一定要认真确认，是否这些代码真的是多余的。

https://developer.android.com/build/shrink-code

http://blog.csdn.net/guolin_blog/article/details/50451259

https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA

When you [enable app optimization](https://developer.android.com/topic/performance/app-optimization/enable-app-optimization), the `isShrinkResources = true` setting instructs the optimizer to remove resources that are unused, which helps reduce the size of your app. Resource shrinking works only in conjunction with code shrinking, so if you're optimizing resources, also set `isMinifyEnabled = true`

```kotlin
android {
    buildTypes {
        release {
            isMinifyEnabled = true
            isShrinkResources = true

            proguardFiles(
                // Default file with default optimization rules.
                getDefaultProguardFile("proguard-android-optimize.txt"),

                // File with your custom rules.
                "proguard-rules.pro"
            )
        }
    }
}
```

# Rules

proguard-android-optimize.txt

preference -> Languages & Frameworks -> Android SDK

Recent Android SDK installations no longer include the traditional `tools` directory by default, replaced with 

```kotlin
val optimizeFile = getDefaultProguardFile("proguard-android-optimize.txt")  
println("Optimize file: ${optimizeFile.absolutePath}")
```

/home/j/AndroidStudioProjects/MyApplication/app/build/intermediates/default_proguard_files/global/proguard-android-optimize.txt-8.3.1

### -allowaccessmodification

- ProGuard can change access modifiers to enable better optimizations
- It can make `private` methods `public` or vice versa if it improves code optimization
- It can change class visibility to allow better dead code elimination

Method Inlining:

```java
// Original code
public class MyClass {
    private void helperMethod() {
        // some code
    }

    public void publicMethod() {
        helperMethod(); // This call can be inlined
    }
}

// With -allowaccessmodification, ProGuard might:
// - Inline helperMethod() directly into publicMethod()
// - Remove helperMethod() entirely if only used once
```

Class Merging:

```
// ProGuard can merge classes more aggressively
// by changing their access modifiers to allow merging
```

Dead Code Elimination:

```
// ProGuard can make unused public methods private
// then eliminate them if they're not used anywhere
```

## Keep rule syntax

```
-<KeepOption> [OptionalModifier,...] <ClassSpecification> [{ OptionalMemberSpecification }] 
```

| Keep option             | Description                                                                                                                     |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| -keep                   | Preserve the class and the members listed in `[{ OptionalMemberSpecification }]`.                                               |
| -keepclassmembers       | Allow optimization of the class; if the class is preserved, preserve the members listed in `[{ OptionalMemberSpecification }]`. |
| -keepnames              | Allow removal of class and members, but don't obfuscate or modify in other ways.                                                |
| -keepclassmembernames   | Allow removal of class and members, but don't obfuscate or modify members in other ways.                                        |
| -keepclasseswithmembers | No removal or obfuscation of classes if members match the specified pattern.                                                    |

### no rule

```kotlin
class TestClass {
    val publicField = "public data"
    private val privateField = "private data"

    fun publicMethod() = "public method called"
    private fun privateMethod() = "private method called"

    fun getPrivateData() = privateField
    fun callPrivateMethod() = privateMethod()
}
```

```java
class KeepTestActivty : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Use TestClass - this makes it reachable, so normally it would be kept
//        val test = TestClass()
//        Log.d("KeepDemo", "Using TestClass: ${test.publicMethod()}")
        // Remove direct usage, use only reflection
        try {
            val clazz = Class.forName("com.example.myapplication.keep.TestClass")
            val instance = clazz.getDeclaredConstructor().newInstance()
            // This will break if TestClass gets obfuscated!
        } catch (e: Exception) {
            Log.e("Test", "Reflection failed: ${e.message}")
        }
}
```

mapping.txt

```
com.example.myapplication.keep.TestClass -> w1.a:
# {"id":"sourceFile","fileName":"TestClass.kt"}
```

otherwise

**ProGuard's logic:**

1. `MainActivity` is kept (Android framework entry point)
2. `MainActivity.onCreate()` calls `TestClass()` constructor → Keep `TestClass`
3. `test.publicMethod()` is called → Keep `publicMethod()`
4. Since class is referenced, ProGuard keeps it to avoid breaking the code

### Keep Entire Class

We recommend using package-wide keep rules as a way to temporarily disable R8 in parts of your app. You should always come back to fix these optimization issues later; this is generally a stop-gap solution to work around problem areas.

For example, if part of your app uses Gson heavily and is causing problems with optimization, the ideal fix is to add more [targeted keep rules](https://developer.android.com/topic/performance/app-optimization/add-keep-rules) or move to a codegen solution. But to unblock optimizing the rest of the app, you can place the code defining your Gson types in a dedicated subpackage, and add a rule like this to your `proguard-rules.pro` file:

```
-keep class com.example.myapplication.keep.TestClass { *; }
```

mapping.txt

```
com.example.myapplication.keep.TestClass -> com.example.myapplication.keep.TestClass:
# {"id":"sourceFile","fileName":"TestClass.kt"}
```

If some library you're using has [reflection](https://en.wikipedia.org/wiki/Reflective_programming) into internal components, you can similarly add a keep rule for the entire library. You'll need to inspect the library's code or JAR/AAR to find the appropriate package to keep. Again, this isn't recommended to maintain long term, but can unblock the optimization of the rest of the app:

```
-keep class com.somelibrary.** { *; }
```

https://developer.android.com/topic/performance/app-optimization/adopt-optimizations-incrementally#enable-rest

### Keep Specific Members

```
-keep class com.example.myapplication.keep.TestClass{
    public java.lang.String publicMethod();
}
```

Decompilation apk got

```
public final class TestClass {
    public static final /* synthetic */ int $r8$clinit = 0;
    public final String publicField = "public data";
    public final String privateField = "private data";

    public final String publicMethod() {
        return "public method called";
    }
}
```

### keepclassmembers

```
-keepclassmembers [,modifier,...] class_specification {
    member_specification
} 
```

#### 

User.java

```java
public class User {
    private String name;
    private int age;
    private String email;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }
    public String getEmail() { return email; }

    public void setName(String name) { this.name = name; }
    public void setAge(int age) { this.age = age; }
    public void setEmail(String email) { this.email = email; }

    private void updateProfile() {

    }
}
```



#### Keep Getter Methods

```
# Keep all getter and setter methods in any class
-keepclassmembers class * {
    public *** get*();
}
```

- Classes can be renamed/optimized
- But if a class survives optimization, its getter/setter methods are preserved
- Method names like `getName()`` won't be obfuscated



Result

```
public class User {
    public int age;
    public String email;
    public String name;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }

    public String getEmail() {
        return this.email;
    }
}
```





#### Keep Fields Used by Serialization

```
# Keep serializable fields
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}
```

#### Keep Enum Values

```
# Keep enum methods that might be called via reflection
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}
```

# Android config

```groovy
buildTypes {
    release {
        isMinifyEnabled = true
        proguardFiles(
            getDefaultProguardFile("proguard-android-optimize.txt"),
            "proguard-rules.pro"
        )
    }
}
```

proguard-android-optimize.txt  in this file

>  /home/mc/Android/Sdk/tools/proguard/proguard-android-optimize.txt

把Apk后缀改成 zip ,解压后把 classes.dex 文件拖到 jadx里面

https://blog.csdn.net/xiaohai695943820/article/details/72367896

https://www.itread01.com/content/1548717866.html

* 反编译工具介绍

https://www.cnblogs.com/zhen-android/p/7830249.html

**脱糖** 即在编译阶段将在语法层面一些底层字节码不支持的特性转换为基础的字节码结构，(比如List上的泛型脱糖后在字节码层面实际为Object)； Android工具链对Java8语法特性脱糖的过程可谓丰富多彩，当然他们的最终目的是一致的：使更新的语法可以在所有的设备上运行。

https://juejin.cn/post/6844903828756643854
