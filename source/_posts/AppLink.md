---
title: AppLink
date: 2022-10-17 16:49:19
tags: 
categories: ANDROID
---



https://juejin.cn/post/7031143982960902157

https://developer.android.com/studio/write/app-link-indexing



### 唤醒App的几种方式



### Deep Link

https://developer.android.com/training/app-links/deep-linking



URL Scheme

**链接格式**

```
[scheme]://[host][:port]/[path]?[query]
```

| 属性名 | 含义                     |
| ------ | ------------------------ |
| scheme | 协议名（由开发人员定义） |
| host   | 网络域名                 |
| port   | 端口号                   |
| path   | 页面路径                 |
| query  | 请求参数                 |



##### scheme only

```xml
<!-- for deep-link -->
<intent-filter>
    <!-- 必须加否否无法响应点击链接的 Intent-->
    <action android:name="android.intent.action.VIEW" />

    <category android:name="android.intent.category.DEFAULT" />

    <category android:name="android.intent.category.BROWSABLE" />

    <data android:scheme="ewallet" />
</intent-filter>
```

在浏览器输入 ewallet:// 就能直接打开app

```
ewallet://
```



#####  scheme host

```xml-dtd
<intent-filter>
    <!-- 必须加否否无法响应点击链接的 Intent-->
    <action android:name="android.intent.action.VIEW" />

    <category android:name="android.intent.category.DEFAULT" />

    <category android:name="android.intent.category.BROWSABLE" />

    <data
        android:host="topic"
        android:scheme="com.great.jon" />
</intent-filter>
```

在浏览器输入

```
com.great.jon://topic
com.great.jon://topic?id =11
```

id=11 这个可以不写



##### ADB TEST

```
adb shell am start -a android.intent.action.VIEW -d "replace your link here"
```





##### Android系统级 Schemes

有一些已经定义了URL Schemes，比如短信是 sms:、通话是tel:、email是mailto:，在定义自己APP的URL Schemes的时候要避免跟系统应用名称一样



##### **各大平台**

简单查找了一下各大平台的 scheme，感兴趣的可以直接从手机浏览器中打开尝试一下。

> QQ：`qq://`
> 微信：`weixin://`
> 淘宝：`taobao://`
> 微博：`sinaweibo://`
> 百瓶：`billionbottle://`

**页面调用方式**

```javascript
window.location.href = schemeUrl;
复制代码
```

直接跳转链接即可，简单方便。

 

##### 负面情况

- 目前没有办法区分多个 App 都注册了相同 scheme 的情况；
- 不支持从其他 App 中的 WebView 直接跳转到目标 App；
- Android 端微信，无法直接通过 scheme 唤起 App，可以通过引导或微信开放标签来解决；
- 只能通过 `hidden`、`blur` 等事件监听到是否安装 App；

因此为了解决以上问题，iOS 和 Android 都有了自己的第二套解决方案，分别是 iOS 的 **Universal Links**，和 Android 的 **App Links**。



### App Links

在 2015 年的 Google I/O 大会上，Android M 宣布了一个新特性：**App Links**。它可以让用户在点击一个普通 Web 链接的时候可以打开指定 App 的对应页面，前提是这个 App 已经安装并且经过了验证，否则会显示一个打开确认选项的对话框，目前只支持 Android M 以上系统。

**App Links** 的出现其实也是为了优化用户体验，在使用它唤醒 App 时会弹出一个对话框提示用户是否打开，缺点就是如果用户勾选了取消之后，可能之后就再也唤醒不了了。

**工作方式及流程**

1. 配置 AndroidManifest.xml；

2. 配置 json 文件；

   ```json
   [{
     "relation": ["delegate_permission/common.handle_all_urls"],
     "target": {
       "namespace": "android_app",
       "package_name": "包名",
       "sha256_cert_fingerprints": ["签名"]
     }
   }]
   ```

3. 将 json 文件上传到指定域名的 .well-known 路径下，文件名定义为  assetlinks.json；

4. 验证 **App Links**，可使用 AndroidStudio 里的 **App Links Assistant** 中的 **Test App Links** 进行测试或者在短信中输入链接点击测试，如果直接唤起 App 没有弹出对话框选择则说明 **App Links** 验证成功；

总的来说与 **Universal Links** 的配置和验证很相似，差异不大。



https://developer.android.com/studio/write/app-link-indexing

http://www.androidchina.net/10135.html



### 第三方服务

##### 微信相关的友情提示

微信已经成为大家日常必不可少的交流工具，用作推广 App 来说是再好不过的，但是微信内部通常是无法直接跳转至其他 App 的。那除了以上列出的技术方案，我们还可以如何去实现这个技术需求呢？

##### 应用宝

如果你的页面需要能直接打开应用商店，可以把你的 App 上传到应用宝平台，因为应用宝和 AppStore 有合作，并且在内部实现了属于自己的一套流程，直接在微信中跳转应用宝的链接也是一个可选的方案。

##### 微信开放标签

微信在 2020 年 5 月推出了微信开放标签功能，用于在微信浏览器内直接唤醒 App，也能通过携带参数直接进入 App 相应的页面，只要按照文档规定接入微信 SDK 就可直接使用该功能。

具体要求可以参考官方开放平台：[微信官方文档](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fdoc%2Foplatform%2FMobile_App%2FWeChat_H5_Launch_APP.html)（ [developers.weixin.qq.com/doc/oplatfo…](https://link.juejin.cn/?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fdoc%2Foplatform%2FMobile_App%2FWeChat_H5_Launch_APP.html) ）。

https://cloud.tencent.com/developer/article/1652487



deeplin VS AppLink

* Deep Links 是一种允许用户进入应用某个特定Activity的intent filter。点击这类链接时，系统可能会弹出一个选择列表，让用户在一堆能够处理这类链接的应用里(包括你的)选择一个来处理该链接。图一展示了这样一种情况：用户点击了一个地图相关的链接，系统弹出一个选择列表，让用户选择是要使用地图应用来处理，还是使用Chrome浏览器来处理。
* App Links 是一种基于你的网站地址且验证通过的Deep Links。因此，点击一个这样的链接会直接打开你的应用(如果已经安装)，系统将不会弹出选择列表。当然，后续用户可以更改配好设置，来指定由哪个应用程序处理这类链接。



| item              | Deep Links                                         | App Links                                                    |
| ----------------- | -------------------------------------------------- | ------------------------------------------------------------ |
| Intent URL Scheme | https, http，或者自定义                            | 需为http或https                                              |
| Intent Action     | 任意Action                                         | 需为`android.intent.action.VIEW`                             |
| Intent Category   | 任意Category                                       | 需为`android.intent.category.BROWSABLE`和`android.intent.category.DEFAULT` |
| 链接验证          | 不需要                                             | 需要在网站上放置一个数字资产链接，并能够通过HTTPS访问        |
| 用户体验          | 可能会弹出一个选择列表给用户选择用哪个应用处理连接 | 没有弹框，系统直接打开你的应用处理网站连接                   |
| 兼容性            | 所有Android版本                                    | Android 6.0及以上                                            |

https://juejin.cn/post/6844903954149539848



由于大部分应用，如微博、微信、第三方浏览器(包括Chrome)，都不会将URL抛给系统处理(对scheme进行屏蔽)，因此App Links生效的情况就很有限了，比如只能从记事本应用、短信应用这些进行跳转。一般商用实现的是打开系统浏览器，通过系统浏览器打开应用的对应页面。



作者：JinBeen
链接：https://juejin.cn/post/6844903954149539848
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

