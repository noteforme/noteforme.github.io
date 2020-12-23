---
title: OKHTTP
comments: true
date: 2017-08-07 10:04:29
tags:
categories: ANDROID

---


##### 如何阅读开源项目

https://time.geekbang.org/column/article/186778

https://www.imooc.com/article/301778

https://baixin.ink/2019/05/10/how-to-learn-opensorce-project/

https://www.jianshu.com/p/656dbb97a40f

https://www.codedump.info/post/20200605-how-to-read-code-v2020/

不管写的怎么样，先把东西弄出来，然后参考别人的写法，做对比



#####  面试题

* [*okhttp* 是如何支持 Http2 的？](https://mp.weixin.qq.com/s/TeQhe4T4wRjdAEPz6Ne45g)

* [Okhttp连接池是咋实现的?](https://juejin.cn/post/6898145227765186567)

* [雨露均沾的OkHttp—WebSocket长连接（使用篇）](https://juejin.im/post/5f0452615188252e5522b747) 

* [*OKHTTP*之缓存配置详解](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650237860&idx=1&sn=d66e75f6f7752ededdcaa3ce780862d3&chksm=88639acbbf1413dd170ba41a67035c62811b489cfc7a405977ae23254205a6b3acb99358b1f2&scene=38#wechat_redirect)

* OkHttp对于网络请求都有哪些优化

* OkHttp框架中都用到了哪些设计模式

  

   https://www.codetd.com/article/4354895

   https://www.jianshu.com/p/d85e556b8da6





#####  解析

1. okhttp 是如何支持 Http2

   Http1.1 http2的区别 https://noteforme.github.io/2017/08/03/Network/



Okhttp缓存

 1、添加cache 路径
 2、初始化OkhttpClient

 ```
   public class NewCacheInterceptor implements Interceptor {

        @Override
        public Response intercept(Chain chain) throws IOException {
            Request request = chain.request();
            Response response = chain.proceed(request);
            Response response1 = response.newBuilder()
                    .removeHeader("Pragma")
                    .removeHeader("Cache-Control")
                    .header("Cache-Control", "max-age=" + 3600 * 24 * 30)
                    .build();

            return response1;
        }
    }

 ```



OkHttp的拦截器有：

- RetryAndFollowUpInterceptor：失败和重定向拦截器；
- BridgeInterceptor：封装Response的拦截器；
- CacheInterceptor：缓存处理相关的拦截器；
- ConnectInterceptor：连接服务的拦截器，真正的网络请求在这里实现；
- CallServerInterceptor：负责写请求和读响应的拦截器；



https://juejin.cn/post/6873476209737629709/


http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0106/2275.html

http://blog.csdn.net/briblue/article/details/52920531
http://mushuichuan.com/2016/03/01/okhttpcache/

https://www.mocklab.io/blog/which-java-http-client-should-i-use-in-2020/



