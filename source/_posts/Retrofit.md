---
title: Retrofit
comments: true
date: 2017-09-23 09:20:09
tags: 
categories: ANDROID
---


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
http://wuxiaolong.me/2016/06/18/retrofits/
