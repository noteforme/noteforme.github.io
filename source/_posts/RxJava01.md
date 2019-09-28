---
title: RxJava01
comments: true
date: 2018-03-28 17:58:39
tags: RxJava
categories: ANDROID
---

##### RxJava

###### Describe

- subscribeOn() 指定上游发送事件的线程, observeOn　指定的是下游接收事件的线程
- 多次指定上游的线程只有第一次指定的有效,就是subscribeOn()只有第一次有效,其余忽略
- 可以多次指定下游线程,每调用一次observeOn(),下游的线程就会切换一次

Demo在　AndroidDemo -> RXLeran->RX基础->Rx线程切换



###### 链式调用

```
  Observable<BaseCount<List<ConfigEnv>>> envApi = RetrofitFactory.configRetrofitService(IApiStores.class).getUnitList();
        RetrofitFactory.configRetrofitService(IApiStores.class).getAppConfig(GsonUtil.loginnoDoctorIdBody(configMap))
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())  //回到主线程去处理请求注册结果
                .doOnNext(new Consumer<BaseCount<SigleField>>() {
                    @Override
                    public void accept(BaseCount<SigleField> sigleFieldBaseCount) throws Exception {

                    }
                })
                .observeOn(Schedulers.io())
                .flatMap(new Function<BaseCount<SigleField>, ObservableSource<BaseCount<List<ConfigEnv>>>>() {
                    @Override
                    public ObservableSource<BaseCount<List<ConfigEnv>>> apply(BaseCount<SigleField> sigleFieldBaseCount) throws Exception {
                        if (sigleFieldBaseCount.getMeta().getStatusCode()==0){
//                            return Observable.error(new Exception());
                        }
                        return envApi;
                    }
                })
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<BaseCount<List<ConfigEnv>>>() {
                    @Override
                    public void accept(BaseCount<List<ConfigEnv>> listBaseCount) throws Exception {
                        Timber.i(" " + listBaseCount.getCount());
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        Timber.i(throwable.getMessage());
                    }
                });
```



http://reactivex.io/documentation/operators.html

https://www.jianshu.com/p/fa1828d70192

https://www.cnblogs.com/fuyaozhishang/p/8697404.html



###### Sample

[水管系列](http://www.10tiao.com/html/169/201709/2650823932/1.html) 

https://github.com/ReactiveX/RxAndroid
https://github.com/amitshekhariitbhu/RxJava2-Android-Samples
https://github.com/rengwuxian/RxJavaSample

https://github.com/ssseasonnn/RxJava2Demo

https://juejin.im/post/5b8f536c5188255c352d3528

[RxJava 只看这一篇文章就够了(上)](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649492706&idx=1&sn=d7d213a1db9c8ae3a5b0525d45863518&chksm=8eec871db99b0e0bc4d4d1aa2b7ed5d7c32e5299aee0f0818a798c2deb2996f40f8971c7a6a2&scene=38#wechat_redirect)

[RxJava 只看这一篇文章就够了 (中)](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649492715&idx=1&sn=9d2160fae874472f1ecf40f91c52d837&chksm=8eec8714b99b0e02f08afc916f94fb2bd075c9ce31c214c5c45674739ca110bed423e8f44d5c&scene=38#wechat_redirect)

[RxJava 只看这一篇文章就够了 (下)](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649492715&idx=2&sn=18ec403398fc5044732779ca8cbddd1b&chksm=8eec8714b99b0e02692798393314d6ac437db33e067ab2880972c1124937a66b32d07b49f978&scene=38#wechat_redirect)



使用MVP Dagger2 https://juejin.im/post/5d5ce44d5188252231108e68

