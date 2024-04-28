---
title: Retrofit
comments: true
date: 2017-09-23 09:20:09
tags: 
categories: ANDROID
---

https://github.com/pengMaster/BestNote/blob/master/docs/android/Android-Interview/Android/Android%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%9510%E5%A4%A7%E5%BC%80%E6%BA%90%E6%A1%86%E6%9E%B6%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90.md


#### Okhttp缺陷
1. 用户网络请求的借口配置 繁琐， 尤其是需要配置复杂的请求body,请求头，参数的时候。
2. 数据解析过程需要用户手动拿到respponsbody进行解析，不能复用。
3. 无法适配自动进行线程的切换。
4. 万一我们的存在嵌套网络请求就会陷入，"回调陷阱".


![2021-07-28_7.46.04.png](Retrofit/e81f0a50834b2fb1b72fb6b7a2f92ec9ed09d6f1.png)


* Retrofit底层实现，在okhttp的基础下做了哪些封装

* Retrofit中的Call对象如何转换成okhttp的call对象(这个题目是埋坑的)

* Retrofit设计模式

https://blog.51cto.com/u_15375308/4872022

在retrofit中的泛型是怎么解析的 https://noteforme.github.io/2017/09/23/Retrofit/
​ 编译时注解与运行时注解，为什么retrofit要使用运行时注解？什么时候用运行时注解？


retrofit的了解

1.动态代理创建一个接口的代理类
2.通过反射解析每个接口的注解、入参构造http请求
3.获取到返回的http请求，使用Adapter解析成需要的返回值。


# InterView Answer

   1.  retrofit流程分析
    Retrofit 交付请求给OkHttp主要是，OkHttpCall中处理, 组装数据后，请求方式就是OkHttp官方提供的方式了。
    请求的body是在这里处理的
```
 okhttp3.Request create(@Nullable Object instance, Object[] args) throws IOException {
      handlers[p].apply(requestBuilder, args[p]);
    return requestBuilder
        .get() // 组装body和请求
        .build();
  }
```
    
    3-4 retrofit请求过程7步骤详解
    3-5 静态代理模式讲解
    3-6 动态代理模式讲解
    3-7 retrofit网络通信流程8步骤&7个关键成员变量解析
    3-8 retrofit中builder构建者模式&builder内部类解析
    3-9 retrofit中baseurl／converter／calladapter解析
    3-10 retrofit中build方法完成retrofit对象创建流程解析
    3-11 retrofit中RxjavaCallAdapterFactory内部构造与工作原理解析
    3-12 retrofit中网络请求接口实例解析
    3-13 retrofit中serviceMethod对象解析
    3-14 retrofit中okHttpCall对象和adapt返回对象解析
    3-15 retrofit中同步请求&重要参数解析
    3-16 retrofit中异步请求解析

# DESIGN PATTERN

![image](https://github.com/noteforme/noteforme.github.io/assets/6995071/fb78a8b1-5458-4d7b-b7f3-e013793379e6)

    
*  retrofit设计模式解析-1：构建者模式  RequestBuilder 
*  retrofit设计模式解析-2：工厂模式
   ## 简单工厂模式
   
   ```
   private static Platform findPlatform() {
    if (isAndroid()) {
      return findAndroidPlatform();
    } else {
      return findJvmPlatform();
    }
  }
   ```
  
    
    
    
*   3-19 retrofit设计模式解析-3：外观模式
    3-20 retrofit设计模式解析-4：策略模式
    3-21 retrofit设计模式解析-5：适配器模式
    3-22 retrofit设计模式解析-6：动态代理模式／观察者
    3-23 retrofit面试题：retfrofit线程切换（异步机制Looper)
    3-24 retrofit面试题：rxjava和retrofit如何结合进行网络请求
    3-25 retrofit面试题：Hook与动态代理
    3-28 retrofit面试题：sp跨进程&apply和commit方法

https://juejin.cn/post/6879326343667023879
https://www.jianshu.com/p/435a5296ee94




#### Retofit调用



![2021-07-27_10.42_retrofit.png](Retrofit/f2f4fc41e9bde83f0f4ff0f6661a3a3de8a8deed.png)





#### Retrofit Rxjava

![2021-07-27_10.43_rxjava.png](Retrofit/e60bb6dbf6f6ec0cb9f6ede98003154818a906f4.png)

![2021-07-28_3.02.05.png](Retrofit/5d1ba3a67f83cdc0a03694e431bee4cda35c51a7.png)



![2021-07-28_04.21.png](Retrofit/00e0ce6a69d77c815471a84f7f9eb221a6900951.png)



 Retorift使用过程

1. 动态生成了实现了接口类型的， 类的对象。
   
   调用方法,通过反射解析接口，拿到方法参数

   生成代理类

   Retrofit.java

```java
public <T> T create(final Class<T> service) {
  validateServiceInterface(service);
  return (T)
      Proxy.newProxyInstance(
          service.getClassLoader(),
          new Class<?>[] {service},
          new InvocationHandler() {
            private final Platform platform = Platform.get();
            private final Object[] emptyArgs = new Object[0];

            @Override
            public @Nullable Object invoke(Object proxy, Method method, @Nullable Object[] args)
                throws Throwable {
              // If the method is a method from Object then defer to normal invocation.
              if (method.getDeclaringClass() == Object.class) {
                return method.invoke(this, args); //传入的是this
              }
              args = args != null ? args : emptyArgs;
              return platform.isDefaultMethod(method)
                  ? platform.invokeDefaultMethod(method, service, proxy, args)
                  : loadServiceMethod(method).invoke(args); //这个动态代理的形式，为什么没传对象？
            }
          });
}
```

```java
IApiStores iApiStores = RetrofitFactory.create(IApiStores.class);
retrofit2.Call<List<SharedListBean>> sharedListCall = iApiStores.getSharedList(2, 1);
```

2. ServiceMethod
   
   一个方法对应一个serviceMethod,使用hashmap缓存,构建okhttp的网络请求。

#### Retrofit包装类

Retrofit组装完数据，使用Okhttp进行网络请求的类

OkHttpCall.java

```java
@Override
public void enqueue(final Callback<T> callback) {
  Objects.requireNonNull(callback, "callback == null");

  okhttp3.Call call;
  Throwable failure;
  synchronized (this) {
    call = rawCall = createRawCall();     // 创建OkhttpCall
  }

  call.enqueue(
      new okhttp3.Callback() {
        @Override
        public void onResponse(okhttp3.Call call, okhttp3.Response rawResponse) {
          Response<T> response;
          try {
            response = parseResponse(rawResponse);
          } catch (Throwable e) {
            throwIfFatal(e);
            callFailure(e);
            return;
          }

          try {
            callback.onResponse(OkHttpCall.this, response);
          } catch (Throwable t) {
            throwIfFatal(t);
            t.printStackTrace(); // TODO this is not great
          }
        }

        @Override
        public void onFailure(okhttp3.Call call, IOException e) {
          callFailure(e);
        }

        private void callFailure(Throwable e) {
          try {
            callback.onFailure(OkHttpCall.this, e);
          } catch (Throwable t) {
            throwIfFatal(t);
            t.printStackTrace(); // TODO this is not great
          }
        }
      });
}
```



https://www.bilibili.com/video/BV1mU4y1p71g?p=10&spm_id_from=pageDriver
https://www.bilibili.com/video/BV12Q4y1d7uD?p=7&spm_id_from=pageDriver
http://www.jianshu.com/p/308f3c54abdd
这篇文章不错，深入浅出
我还加了了提交Map参数的[Demo](https://gitee.com/huaiyi/RetrofitDemo.git) Demo

 http://www.10tiao.com/html/227/201701/2650238307/1.html
 http://blog.csdn.net/itjianghuxiaoxiong/article/details/52135748

https://juejin.im/post/5afc1706518825426f30f6ec

要实现类似这样的请求,用post方式怎么也不行
https://newsapi.org/v2/top-headlines?sources=financial-times&apiKey=e4f505f73a9f4ee99119ab33a19ab05e




问1：什么是动态代理？
动态代理和静态代理都属于代理模式，动态代理是可以在运行期动态创建某个interface的实例，我们通过Proxy.newProxyInstance产生的代理类，当调用接口的任何方法时，都会被InvocationHandler#invoke方法拦截，同时，在这个方法中可以拿到所传入的参数等，依照参数值再做相应的处理。

问2：Retrofit是如何将子线程切换到主线程？
在添加默认适配器工厂defaultCallAdapterFactories时，将callbackExecutor作为了一个参数，那么它的具体实现也就是在这个默认适配器工厂中。 我们来看下callbackExecutor在里面做了些啥。

static final class ExecutorCallbackCall<T> implements Call<T> {
    final Executor callbackExecutor;
    final Call<T> delegate;
    ...
 
    @Override
    public void enqueue(final Callback<T> callback) {
 
      delegate.enqueue(
          new Callback<T>() {
            @Override
            public void onResponse(Call<T> call, final Response<T> response) {
              callbackExecutor.execute(
                  () -> {
                    if (delegate.isCanceled()) {
                      // Emulate OkHttp's behavior of throwing/delivering an IOException on
                      // cancellation.
                      callback.onFailure(ExecutorCallbackCall.this, new IOException("Canceled"));
                    } else {
                      callback.onResponse(ExecutorCallbackCall.this, response);
                    }
                  });
            }
 
            @Override
            public void onFailure(Call<T> call, final Throwable t) {
              callbackExecutor.execute(() -> callback.onFailure(ExecutorCallbackCall.this, t));
            }
          });
    }
 

在上述代码里了解到，callbackExecutor即Executor，一个线程调度器。在Call的enqueue实现里执行了一个异步网络请求delegate.enqueue，在请求的响应onResponse、onFailure中 Executor也同样执行了一个线程，这里就有个疑问，为什么要在一个异步请求里又调用一个线程？我们知道callbackExecutor是一个线程调度器，那他内部到底实现的是什么？ 默认callbackExecutor的创建在Retrofit的初始化中，callbackExecutor = platform.defaultCallbackExecutor();

static final class Android extends Platform {
 
    @Override
    public Executor defaultCallbackExecutor() {
      return new MainThreadExecutor();
    }
 
    static final class MainThreadExecutor implements Executor {
      private final Handler handler = new Handler(Looper.getMainLooper());
 
      @Override
      public void execute(Runnable r) {
        handler.post(r);
      }
    }
  }
}

platform是一个Android平台，defaultCallbackExecutor 内部其实调用的是 new MainThreadExecutor() ，很清楚的看到， handler.post(r) 内部使用Handler将响应抛到了主线程。

这就是Retrofit将子线程切换到主线程的核心所在。

问3：Retrofit为什么要用动态代理？

                        
原文链接：https://blog.csdn.net/qq_37492806/article/details/133995368


Retrofit结合RxJava 感觉RxJava可以了解下，
Retrofit结合courutine 感觉courutine代码不懂，courutine的可以了解下，感觉courutine有先级别更高一点，回来再来画Retrofit其他的类图








