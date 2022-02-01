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
  RetrofitFactory.create(ICommonApis.class)
                .imgUpServer(part)
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .doOnNext(new Consumer<PictureUp>() {
                    @Override
                    public void accept(PictureUp pictureUp) throws Exception {
                        List<PictureUp.ModelListBean> picList = pictureUp.getModel_list();
                        if (picList != null && !picList.isEmpty()) {
                            String feet_id = picList.get(0).getFile_group_id();
                            //上传成功
                            if (!isEmpty(feet_id) && mvpView != null) {
                                mvpView.upFeetId(feet_id);
                            }
                        }
                    }
                })
                .observeOn(Schedulers.io())
                .flatMap(new Function<PictureUp, ObservableSource<BaseCount>>() {
                    @Override
                    public ObservableSource<BaseCount> apply(PictureUp pictureUp) throws Exception {
                        List<PictureUp.ModelListBean> picList = pictureUp.getModel_list();
                        if (picList != null && !picList.isEmpty()) {
                            String feet_id = picList.get(0).getFile_group_id();
                            //上传成功
                            if (!isEmpty(feet_id) && mvpView != null) {
                                mvpView.upFeetId(feet_id);
                            }else {
                                return  Observable.error(new Exception());
                            }
                        }
                        return RetrofitFactory.create(ICommonApis.class).offlineBoxInfo(requestBody);
                    }
                })
                .subscribe(new Consumer<BaseCount>() {
                    @Override
                    public void accept(BaseCount baseCount) throws Exception {
                        if (mvpView == null) {
                            return;
                        }
                        if (baseCount.getMeta().getStatusCode() == AUTH_FAILED) {
                            mvpView.unOfficeEquip(baseCount);
                        } else if (baseCount.getMeta().getStatusCode() == 0) {
                            mvpView.upSuccess();
                        } else {
                            if (baseCount.getMeta() != null && !isEmpty(baseCount.getMeta().getDescribe())) {
                                ToastUtil.showBiggerText(baseCount.getMeta().getDescribe());
                            }
                        }
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        if (mvpView != null) {
                            mvpView.upUserInfoFailed();
                        }
                    }
                });
```



```
fun getImgScan() {
    val request = RetrofitWeChatFactory.create(IApiStore::class.java)
    request.get_access_token()
        .flatMap { request.getTicket(it.access_token, 2) }
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(object : Observer<WeChatSecond> {
            override fun onComplete() {
            }

            override fun onSubscribe(d: Disposable) {
            }

            override fun onNext(t: WeChatSecond) {
                val noncestr = LoginNewActivity.getRandomString(8)
                val timeStamp =
                    (System.currentTimeMillis() / 1000).toString()
                t.ticket?.let {
                    val string1 = String.format(
                        "appid=%s&noncestr=%s&sdk_ticket=%s&timestamp=%s",
                        WeChatAppID,
                        noncestr,
                        it,
                        timeStamp
                    )
                    val sha = EncryptUtils.getSHA(string1)

                    mvpView.sign(noncestr, timeStamp, sha)
                }
            }

            override fun onError(e: Throwable) {
            }

        })
}
```

[http://yamlee.me/2020/03/11/2020-03-11-%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3RxJava%E4%B8%8ERxKotlin/](http://yamlee.me/2020/03/11/2020-03-11-深入理解RxJava与RxKotlin/)



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

https://juejin.im/user/590210f4ac502e0063d338f5/posts