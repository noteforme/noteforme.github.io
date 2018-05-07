---
title: SDK_Integration
comments: true
date: 2017-09-19 14:44:01
tags:
categories: 工具

---

平常集成SDK虽然简单，但是过了半年就全忘了，又得重新来过

## 友盟分享

[微信开发者平台](https://open.weixin.qq.com/)
创建应用需要 **应用签名**

需要签名工具,可以从 [微信资源中心](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419319167&token=&lang=zh_CN) 获取

 注意要打包才能运行



##  商汤认证

[文档](https://v2-devcenter.visioncloudapi.com/#!/home/doc/ocr/android/online/useguide)

###  身份证认证

* 在common-idcard包中的assets目录下添加 SenseID_OCR.lic文件就可以了

* 在androidmanifest.xml注册activity

  ​


### 活体认证

最终调用onDetectOver

```
  public void onDetectOver(ResultCode code, String id, List imageData)
```

会返回id,然后把id,身份证number,name传给商汤对比



##  极光推送

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






