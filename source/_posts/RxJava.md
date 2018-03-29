---
title: RxJava
comments: true
date: 2018-03-28 17:58:39
tags: RxJava
categories: ANDROID
---

## RxJava Retrofit 简单使用

http://www.jianshu.com/p/56f15db86ed3

## RxJava理解

- subscribeOn() 指定上游发送事件的线程, observeOn　指定的是下游接收事件的线程
- 多次指定上游的线程只有第一次指定的有效,就是subscribeOn()只有第一次有效,其余忽略
- 可以多次指定下游线程,每调用一次observeOn(),下游的线程就会切换一次

Demo在　AndroidDemo -> RXLeran->RX基础->Rx线程切换

参考: http://www.jianshu.com/p/8818b98c44e2

- Scheduler.io()



## 常用方式

https://github.com/ReactiveX/RxAndroid
https://github.com/amitshekhariitbhu/RxJava2-Android-Samples
https://github.com/rengwuxian/RxJavaSamples





# RxJava2学习

来自[鸿洋公众号的教程](http://www.10tiao.com/html/169/201709/2650823932/1.html) 

flatMap

```
api.register(new RegisterRequest())            //发起注册请求
                .subscribeOn(Schedulers.io())               //在IO线程进行网络请求
                .observeOn(AndroidSchedulers.mainThread())  //回到主线程去处理请求注册结果
                .doOnNext(new Consumer<RegisterResponse>() {
                    @Override
                    public void accept(RegisterResponse registerResponse) throws Exception {
                        //先根据注册的响应结果去做一些操作
                    }
                })
                .observeOn(Schedulers.io())                 //回到IO线程去发起登录请求
                .flatMap(new Function<RegisterResponse, ObservableSource<LoginResponse>>() {
                    @Override
                    public ObservableSource<LoginResponse> apply(RegisterResponse registerResponse) throws Exception {
                        return api.login(new LoginRequest());
                    }
                })
                .observeOn(AndroidSchedulers.mainThread())  //回到主线程去处理请求登录的结果
                .subscribe(new Consumer<LoginResponse>() {
                    @Override
                    public void accept(LoginResponse loginResponse) throws Exception {
                        Toast.makeText(MainActivity.this, "登录成功", Toast.LENGTH_SHORT).show();
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        Toast.makeText(MainActivity.this, "登录失败", Toast.LENGTH_SHORT).show();
                    }
                });


```

参考: https://www.jianshu.com/p/128e662906af

源码 : https://github.com/ssseasonnn/RxJava2Demo



* retryWhen

 [[泽毛](https://www.jianshu.com/u/37baa8a86582)]  https://www.jianshu.com/p/fa1828d70192