---
title: SDK_Integration
comments: true
date: 2017-09-19 14:44:01
tags:
categories: TOOL

---

平常集成SDK虽然简单，但是过了半年就全忘了，又得重新来过

#### 友盟分享

[微信开发者平台](https://open.weixin.qq.com/)
创建应用需要 **应用签名**

需要签名工具,可以从 [微信资源中心](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419319167&token=&lang=zh_CN) 获取

 注意要打包才能运行

####  商汤认证

[文档](https://v2-devcenter.visioncloudapi.com/#!/home/doc/ocr/android/online/useguide)

身份证认证

* 在common-idcard包中的assets目录下添加 SenseID_OCR.lic文件就可以了

* 在androidmanifest.xml注册activity

  

活体认证

最终调用onDetectOver

```
  public void onDetectOver(ResultCode code, String id, List imageData)
```

会返回id,然后把id,身份证number,name传给商汤对比

#### 极光推送

* 集成过程

  按照文档，主要是要注意这个广播接收器就可以了

  ```
    <receiver
              android:name=".jpush.MyReceiver"
              android:enabled="true">
              <intent-filter>
                  <!--Required 用户注册SDK的intent-->
                  <action android:name="cn.jpush.android.intent.REGISTRATION" />
                  <!--Required 用户接收SDK消息的intent-->
                  <action android:name="cn.jpush.android.intent.MESSAGE_RECEIVED" />
                  <!--Required 用户接收SDK通知栏信息的intent-->
                  <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED" />
                  <!--Required 用户打开自定义通知栏的intent-->
                  <action android:name="cn.jpush.android.intent.NOTIFICATION_OPENED" />
                  <!-- 接收网络变化 连接/断开 since 1.6.3 -->
                  <action android:name="cn.jpush.android.intent.CONNECTION" />
                  <category android:name="com.zhujia.land" />
              </intent-filter>
          </receiver>
  ```

  

* 有点疑惑的是推送给特定的APP

  目前获取RegistrationID，登陆接口把 RegistrationID传给服务端, 然后Server推送过来就可以了

  http://blog.jiguang.cn/push_audience_tech-2/

   暂时别试用Jcenter集成,会出现各种问题



#### 微信登陆

#####  App登录

https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Resource_Center_Homepage.html

1. 问题1

`E/MtaSDK: [pool-4-thread-1(320): null:705] - Server response error code:404, error:{"ret":-1, "msg":"invalid appkey"}`

Demo里的wechat-sdk-android-with-mta 换成     api 'com.tencent.mm.opensdk:wechat-sdk-android-without-mta:+'  这个弄了一上午

2. 问题2  

    ` No value for openid.` WXEntryActivity {"errcode":40029,"errmsg":"invalid code"}   WXEntryActivity
    appid secret错误

   ```
   if (resp.getType() == ConstantsAPI.COMMAND_SENDAUTH) {
      SendAuth.Resp authResp = (SendAuth.Resp)resp;
      final String code = authResp.code;
      NetworkUtil.sendWxAPI(handler, String.format("https://api.weixin.qq.com/sns/oauth2/access_token?" +
                  "appid=%s&secret=%s&code=%s&grant_type=authorization_code", "wx930ea5d5a258f4f",
            "1d6d1d57a3dd06336d917bc0b44d964", code), NetworkUtil.GET_TOKEN);
   }
   ```

   ```text
   GET https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code
   ```

   demo中 

   appid wxd930ea5d5a2584f要换成自己的

   secret 1d6d1d57a3dd063b36d91bc0b44d964也要换成自己的

   3. 问题3  需要注册，否则WXEntryActivity不能收到回调

      ```java
      private static final String APP_ID = "wx88888888";
      
      // IWXAPI 是第三方app和微信通信的openApi接口
      private IWXAPI api;
      
      private regToWx() {
          // 通过WXAPIFactory工厂，获取IWXAPI的实例
          api = WXAPIFactory.createWXAPI(this, APP_ID, true);
      
          // 将应用的appId注册到微信
          api.registerApp(APP_ID);
      
         //建议动态监听微信启动广播进行注册到微信
        registerReceiver(new BroadcastReceiver() {
         @Override
         public void onReceive(Context context, Intent intent) {
      
           // 将该app注册到微信
          api.registerApp(Constants.APP_ID);
         }
        }, new IntentFilter(ConstantsAPI.ACTION_REFRESH_WXAPP));
      
      }
      ```

   

   ##### 二维码扫码登录
   
   https://developers.weixin.qq.com/doc/oplatform/Mobile_App/WeChat_Login/Login_via_Scan.html
   
   1. 登录公众平台 https://mp.weixin.qq.com/cgi-bin/home?t=home ,点击配置进入IP白名单

      点击配置进入IP白名单设置页
   
      https://mp.weixin.qq.com/cgi-bin/announce?action=getannouncement&key=1495617578&version=1&lang=zh_CN&platform=2&token=38764305
   
   2. 二维码扫码登录的appid secret key用开放平台的  https://open.weixin.qq.com/cgi-bin/index?t=home/index&lang=zh_CN
   
      
      注意两个不同的平台配置



二维码失效:  首先普及一下二维码是有失效时间以及失效状态的，一旦你扫过一次二维码或者在某段时间内没有扫描页面上的二维码，该二维码就失效了



app分享拉新规则

https://mp.weixin.qq.com/s/3bsmiv78yJ4XKwd8iu-0Ig



#### Wechat Pay

https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_5

#### Alipay

https://opendocs.alipay.com/open/204/105296

orderInfo订单信息 由接口拼接就可以了



https://mp.weixin.qq.com/s/wWB5ENo3eQJH03OXvoup8w

https://mp.weixin.qq.com/s?__biz=MzUxODg0MzU2OQ==&mid=2247485318&idx=1&sn=174338566ad8c0426c95815c4f4aee5a&scene=21#wechat_redirect



推送相关

https://juejin.im/post/6881414546859229192



