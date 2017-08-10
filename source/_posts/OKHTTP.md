---
title: OKHTTP
comments: true
date: 2017-08-07 10:04:29
tags:
categories:

---
不管写的怎么样，先把东西弄出来，然后参考别人的写法，做对比

先看看标准化的写法
http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0106/2275.html
其他的都是根据标准化写法进行封装

## Okhttp缓存
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

http://blog.csdn.net/briblue/article/details/52920531
http://mushuichuan.com/2016/03/01/okhttpcache/

## 封装
http://www.jianshu.com/p/ddbf69d1c9d1