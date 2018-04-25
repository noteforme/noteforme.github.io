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

* 推送给全部人很简单
* 有点疑惑的是推送给特定的APP

http://blog.jiguang.cn/push_audience_tech-2/






