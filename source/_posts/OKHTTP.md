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


okhttp的请求机制



oKhttp：
1.同步和异步：
1.异步使用了Dispatcher来将存储在 Deque 中的请求分派给线程池中各个线程执行。
2.当任务执行完成后，无论是否有异常，finally代码段总会被执行，也就是会调用Dispatcher的finished函数，它将正在运行的任务Call从队列runningAsyncCalls中移除后，主动的把缓存队列向前走了一步。
2.连接池：
1.一个Connection封装了一个socket，ConnectionPool中储存s着所有的Connection，StreamAllocation是引用计数的一个单位
2.当一个请求获取一个Connection的时候要传入一个StreamAllocation，Connection中存着一个弱引用的StreamAllocation列表，每当上层应用引用一次Connection，StreamAllocation就会加一个。反之如果上层应用不使用了，就会删除一个。
3.ConnectionPool中会有一个后台任务定时清理StreamAllocation列表为空的Connection。5分钟时间，维持5个socket
3.选择路线与建立连接
1.选择路线有两种方式：
1.无代理，那么在本地使用DNS查找到ip，注意结果是数组，即一个域名有多个IP，这就是自动重连的来源
2.有代理HTTP：设置socket的ip为代理地址的ip，设置socket的端口为代理地址的端口
3.代理好处：HTTP代理会帮你在远程服务器进行DNS查询，可以减少DNS劫持。
2.建立连接
1.连接池中已经存在连接，就从中取出(get)RealConnection，如果没有命中就进入下一步
2.根据选择的路线(Route)，调用Platform.get().connectSocket选择当前平台Runtime下最好的socket库进行握手
3.将建立成功的RealConnection放入(put)连接池缓存
4.如果存在TLS，就根据SSL版本与证书进行安全握手
5.构造HttpStream并维护刚刚的socket连接，管道建立完成
4.职责链模式：缓存、重试、建立连接等功能存在于拦截器中网络请求相关，主要是网络请求优化。网络请求的时候遇到的问题
5.博客推荐：**Android数据层架构的实现 上篇、Android数据层架构的实现 下篇**



okio
1.简介；
1.sink：自己–》别人
2.source：别人–》自己
3.BufferSink：有缓存区域的sink
4.BufferSource：有缓存区域的source
5.Buffer：实现了3、4的缓存区域，内部有Segment的双向链表，在在转移数据的时候，只需要将指针转移指向就行
2.比java io的好处：
1.减少内存申请和数据拷贝
2.类少，功能齐全，开发效率高
3.内部实现：
1.Buffer的Segment双向链表，减少数据拷贝
2.Segment的内部byte数组的共享，减少数据拷贝
3.SegmentPool的共享和回收Segment
4.sink和source中被实际操作的其实是Buffer，Buffer可以充当sink和source
5.最终okio只是对java io的封装，所有操作都是基于java io 的
写在最后:能看到这里的人,我挺佩服你的.这篇文章是我在头条面试之前整理的,最后**80%**的题目都命中了,所以祝你好运.

不贩卖焦虑，也不标题党。分享一些这个世界上有意思的事情。题材包括且不限于：科幻、科学、科技、互联网、程序员、计算机编程。下面是我的微信公众号：世界上有意思的事，干货多多等你来看。

作者：何时夕
链接：https://www.jianshu.com/p/cf5092fa2694
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


OkHttp线程池

ThreadPoolExecutor executor = new ThreadPoolExecutor(
        0, Integer.MAX_VALUE, 60, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
可以看到 corePoolSize 0 , MaxPoolSize Integer.MAX_VALUE

根据线程池执行流程：

首先核心线程，corePoolSize 为0 。
把任务加入SynchronousQueue，但是这个队列加入就会失败。
创建非核心线程，数量为Integer.MAX_VALUE，可以创建。
当任务执行完后，3创建的非核心线程 根据keepAliveTime时间，逐步销毁。


问题
OkHttpThreadPool.java


ThreadPoolExecutor executor = new ThreadPoolExecutor(
        0, Integer.MAX_VALUE, 60, TimeUnit.SECONDS, new LinkedBlockingDeque<>());
executor.execute(() -> {
    System.out.println("任务1");
    System.out.println(Thread.currentThread());
    while (true) {

    }
});

executor.execute(() -> {
    System.out.println("任务1");
    System.out.println(Thread.currentThread());
});

executor.execute(() -> {
    System.out.println("任务1");
    System.out.println(Thread.currentThread());
});
运行结果:

任务1
Thread[pool-1-thread-1,5,main]

如果 new LinkedBlockingDeque<>(1)能正常执行，因为LinkedBlockingDeque加入 任务1 就满了，后面的任务创建非核心线程

但是有点疑惑，我这里核心线程是0，任务都加入到LinkedBlockingDeque, 按照线程池流程，非核心就不应该创建呀？怎么任务1就执行了呢

https://www.bilibili.com/video/BV15y4y1B7Rw?p=5

https://mp.weixin.qq.com/s/BHDrSgwUVXkzvswK1khidQ

https://github.com/Snailclimb/programmer-advancement

https://juejin.im/post/6855586076132655118

https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485441&idx=1&sn=303a25ab02fa9f14a319923e6b0d9759&chksm=cea247caf9d5cedc3a5e1d31f26c08d8ae4c11c349fbdc91ac1d90d8b35807517accb5f5d527&token=2128752750&lang=zh_CN#rd


okhttp
​ https://noteforme.github.io/2017/08/07/OKHTTP/

okhttp怎么支持http2.0
​ Handshake则会把服务端支持的Tls版本，加密方式等都带回来，然后会把这个没有验证过的HandShake用X509Certificate去验证证书的有效性。然后会通过Platform去从SSLSocket去获取ALPN的协议支持信息，当后端支持的协议内包含Http2.0时，则就会把请求升级到Http2.0阶段。

​ 配置合适的适配器，解析json数据。

​ Android 如何编写基于编译时注解的项目

​ 编译时注解与运行时注解，为什么retrofit要使用运行时注解？什么时候用运行时注解？

​ 在项目中有直接使用tcp,socket来发送消息吗

​ https://www.bilibili.com/video/BV1ib4y1f7S1

Okhttp缓存机制

网络请求缓存处理，okhttp如何处理网络缓存的
自己去设计网络请求框架，怎么做？

从网络加载一个10M的图片，说下注意事项
TCP的3次握手和四次挥手
TCP与UDP的区别
TCP与UDP的应用
HTTP协议
HTTP1.0与2.0的区别
HTTP报文结构
HTTP与HTTPS的区别以及如何实现安全性
如何验证证书的合法性?
https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
client如何确定自己发送的消息被server收到?
谈谈你对安卓签名的理解。
请解释安卓为啥要加签名机制?
视频加密传输
App 是如何沙箱化，为什么要这么做？
权限管理系统（底层的权限是如何进行 grant 的）？


 glide和OkHttp的任务调度是怎么实现的（比如同时发起很多请求）
 4.问第三方库如okhttp、picasso等底层原理如缓存机制等（一个也没答上来，literally

13.Retrofit中的Call对象如何转换成okhttp的call对象(这个题目是埋坑的)
11.okhttp责任链设计模式
6.okhttp发送请求的拦截方式
7.okhttp的拦截器设计模式



