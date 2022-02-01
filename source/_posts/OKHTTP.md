---
title: OKHTTP
comments: true
date: 2017-08-07 10:04:29
tags:
categories: ANDROID

---



Okhttp使用线程池无 核心级线程，不应该先把阻塞队列塞满，再执行非核心级线程吗，这样任何不应该会立即执行呀？？？

阻塞队列用了synchrnized 会添加失败。所以直接创建非核心线程。



#### 如何阅读开源项目

https://time.geekbang.org/column/article/186778

https://www.imooc.com/article/301778

https://baixin.ink/2019/05/10/how-to-learn-opensorce-project/

https://www.jianshu.com/p/656dbb97a40f

https://www.codedump.info/post/20200605-how-to-read-code-v2020/

不管写的怎么样，先把东西弄出来，然后参考别人的写法，做对比



#### Okhttp优势

https://square.github.io/okhttp/



OkHttp is an HTTP client that’s efficient by default:

- HTTP/2 support allows all requests to the same host to share a socket.
- Connection pooling reduces request latency (if HTTP/2 isn’t available).
- Transparent GZIP shrinks download sizes.
- Response caching avoids the network completely for repeat requests.





#### 任务运行图

![](OKHTTP/2021-07-23_2.26_OKHTTP.png)





#### 分析

一开始看了网上视频，就说Okhttp是自驱动循环调用，相对于AsyncTask的优势就是 并发执行，但是这两条不就矛盾了吗，既然环形链式调用，怎么能并发呢。就从源码中找答案。



Dispatcher.java

```java
private int maxRequests = 64;
private int maxRequestsPerHost = 5;
synchronized void enqueue(AsyncCall call) {
    if (runningAsyncCalls.size() < maxRequests && runningCallsForHost(call) < maxRequestsPerHost) {
      runningAsyncCalls.add(call);
      executorService().execute(call);
    } else {
      readyAsyncCalls.add(call);
    }
}
```



可以看到提交任务 >5时，才会被添加到readyAsyncCalls队列中。<5的任务直接提交。

```java
  private void promoteCalls() {
    if (runningAsyncCalls.size() >= maxRequests) return; // Already running max capacity.
    if (readyAsyncCalls.isEmpty()) return; // No ready calls to promote.

    for (Iterator<AsyncCall> i = readyAsyncCalls.iterator(); i.hasNext(); ) {
      AsyncCall call = i.next();

      if (runningCallsForHost(call) < maxRequestsPerHost) {
        i.remove();
        runningAsyncCalls.add(call);
        Log.i("Dispatcher", "promoteCalls:  准备队列 "+call.request().tag+" 执行");
        executorService().execute(call);
      }

      if (runningAsyncCalls.size() >= maxRequests) return; // Reached max capacity.
    }
  }
```

readyAsyncCalls不为空，然后取出一条，再执行，可以看到，默认情况下会有5条环形任务链。



#### 拦截器

![](OKHTTP/2021-07-24_6.43_interupt.png)



OkHttp的拦截器有：

- RetryAndFollowUpInterceptor：失败和重定向拦截器；
- BridgeInterceptor：负责将http协议必备的请求头加入其中(host),并添加一些默认的行为(gzip),获得结果后，调用cookie接口并解析GZIP数据。
- CacheInterceptor：缓存处理相关的拦截器；
- ConnectInterceptor： 负责找到或者新建一个连接，并获取对应的socket流；在获得结果后不进行额外的处理。
- CallServerInterceptor：进行真正的与服务器的通信，向服务器请求和读响应的拦截器；



#####  面试题

[*okhttp* 是如何支持 Http2 的？](https://mp.weixin.qq.com/s/TeQhe4T4wRjdAEPz6Ne45g)

Handshake则会把服务端支持的Tls版本，加密方式等都带回来，然后会把这个没有验证过的HandShake用X509Certificate去验证证书的有效性。然后会通过Platform去从SSLSocket去获取ALPN的协议支持信息，当后端支持的协议内包含Http2.0时，则就会把请求升级到Http2.0阶段。

[Okhttp连接池是咋实现的?](https://juejin.cn/post/6898145227765186567)

在连接池中找连接的时候会对比连接池中相同host的连接。

如果在连接池中找不到连接的话，会创建连接，创建完后会存储到连接池中。

在把连接放入连接池中时，会把清除操作的任务放入到线程池中执行，删除任务中会判断当前连接有没有在使用中，有没有正在使用通过RealConnection的transmitters集合的size是否为0来判断，如果不在使用中，找出空闲时间最长的连接，如果空闲时间最长的连接超过了keep-alive默认的5分钟或者空闲的连接数超过了最大的keep-alive连接数5个的话，会把存活时间最长的连接从连接池中删除。保证keep-alive的最大空闲时间和最大的连接数



* [雨露均沾的OkHttp—WebSocket长连接（使用篇）](https://juejin.im/post/5f0452615188252e5522b747) 

* [*OKHTTP*之缓存配置详解](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650237860&idx=1&sn=d66e75f6f7752ededdcaa3ce780862d3&chksm=88639acbbf1413dd170ba41a67035c62811b489cfc7a405977ae23254205a6b3acb99358b1f2&scene=38#wechat_redirect)

* OkHttp对于网络请求都有哪些优化

* OkHttp框架中都用到了哪些设计模式

  

   https://www.codetd.com/article/4354895

   https://www.jianshu.com/p/d85e556b8da6





#####  解析

Okhttp缓存

 1、添加cache 路径
 2、初始化OkhttpClient

 ```java
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





https://juejin.cn/post/6873476209737629709/


http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0106/2275.html

http://blog.csdn.net/briblue/article/details/52920531
http://mushuichuan.com/2016/03/01/okhttpcache/

https://www.mocklab.io/blog/which-java-http-client-should-i-use-in-2020/



https://www.bilibili.com/video/BV12Q4y1d7uD?p=7&spm_id_from=pageDriver
