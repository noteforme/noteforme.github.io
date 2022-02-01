---
title: WebView
comments: true
date: 2017-09-01 16:07:15
tags:
categories: ANDROID

---



#### 使用

##### 加载apk包中的html页面
` webView.loadUrl("file:///android_asset/java.html");`




#####  Enabling JavaScript

```
WebView myWebView = (WebView) findViewById(R.id.webview);
WebSettings webSettings = myWebView.getSettings();

   //支持JavaScript
//        webSettings.setJavaScriptEnabled(true); // 在 onStop 和 onResume 里分别把 setJavaScriptEnabled() 给设置成 false 和 true 即可

        /**
         *  设置自适应屏幕,两者合用
         */
        webSettings.setUseWideViewPort(true); //将图片调整到适合webview的大小
        webSettings.setLoadWithOverviewMode(true); //缩放至屏幕的大小

        /**
         * 缩放操作
         */
        webSettings.setSupportZoom(true);  //支持缩放，默认为true。是下面那个的前提。
        webSettings.setBuiltInZoomControls(true); //设置内置的缩放控件。若为false，则该WebView不可缩放
        webSettings.setDisplayZoomControls(false);//隐藏原生的缩放控件

//        myWebView.setWebChromeClient(new MyWebViewClient());
        myWebView.setWebViewClient(new MyViewClient());

```
上面是一些常用的操作

##### Binding JavaScript code to Android code(JS交互)

When developing a web application that's designed specifically for the WebView in your Android application, you can create interfaces between your JavaScript code and client-side Android code. For example, your JavaScript code can call a method in your Android code to display a Dialog, instead of using JavaScript's alert() function.

To bind a new interface between your JavaScript and Android code, call addJavascriptInterface(), passing it a class instance to bind to your JavaScript and an interface name that your JavaScript can call to access the class.

* office Demo

```
   inner class WebLoginInterface(context: Context?) {
        @JavascriptInterface
        fun appLogin() {
            Timber.i("appLogin")
                mWebView?.post {
                    val mSessionId = SharedPreferencesUtils.getParam(AppCtxUtil.getApp(), GsonUtil.SHARE_TOKEN_KEY, "") as String
                    Timber.i("mSessionId ${mSessionId}")
                    val jsMethodName = "javascript:appLoginCallBack('$mSessionId','4')"; //需要参数的JS函数名
                    Timber.i("jsMethodName $jsMethodName")
                    mWebView?.loadUrl(jsMethodName)
                }
        }
    }
```
注意
> Caution: If you've set your targetSdkVersion to 17 or higher, you must add the @JavascriptInterface annotation to any method that you want available to your JavaScript (the method must also be public). If you do not provide the annotation, the method is not accessible by your web page when running on Android 4.2 or higher.




 html小例子
```
<input type="button" value="Say hello" onClick="showAndroidToast('Hello Android!')" />

<script type="text/javascript">
    function showAndroidToast(toast) {
        Android.showToast(toast);
    }
</script>
```


接着加载JS
```
WebView webView = (WebView) findViewById(R.id.webview);
webView.addJavascriptInterface(new WebAppInterface(this), "Android");
```
https://developer.android.com/guide/webapps/webview.html
http://www.jianshu.com/p/3c94ae673e2a

http://www.jianshu.com/p/c20513cad758

#####  缓存方案

##### office (原生缓存)
```
	   
	    WebSettings webSettings = wbNews.getSettings();
        webSettings.setJavaScriptEnabled(true);// 启用支持javascript
        webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);//缓存模式
        webSettings.setDomStorageEnabled(true); // 开启DOM storage API 功能
        // 开启database storage API功能
        webSettings.setDatabaseEnabled(true);
        webSettings.setAllowFileAccess(true);//可以访问文件
        webSettings.setBuiltInZoomControls(true);//支持缩放

        // 设置数据库缓存路径
        String cacheDirPath = getFilesDir().getAbsolutePath() + APP_CACHE_DIRNAME;
        webSettings.setAppCachePath(cacheDirPath);
        webSettings.setAppCacheEnabled(true);
```
http://blog.csdn.net/coder_pig/article/details/48468969

#####  图片替换方式
https://github.com/GcsSloop/diycode/blob/master/blog/journal-02.md



[其他常用方式](https://github.com/yipianfengye/androidProject/blob/master/18%20android%E4%BA%A7%E5%93%81%E7%A0%94%E5%8F%91-webview%E9%97%AE%E9%A2%98%E9%9B%86%E9%94%A6.md) 
[美团点评](https://tech.meituan.com/WebViewPerf.html)
[腾讯](https://mp.weixin.qq.com/s/evzDnTsHrAr2b9jcevwBzA)

<https://juejin.im/post/5a94f9d15188257a63113a74>

#####   页面白屏

```
webSettings.setDomStorageEnabled(true); // 开启DOM storage API 功能
```

​	  

https://blog.csdn.net/ONLYMETAGAIN/article/details/78390643

https://iluhcm.com/2017/12/10/design-an-elegant-and-powerful-android-webview-part-one/

* 不显示图片的问题

  ```
          webSettings.setBlockNetworkImage(false);
          if (Build.VERSION.SDK_INT>=Build.VERSION_CODES.LOLLIPOP){
              webSettings.setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
          }
  ```

  但是我的还是不显示

```
android:hardwareAccelerated="false"
```



WebView缓存功能

https://github.com/yale8848/CacheWebView

[http://blog.leanote.com/post/zbyzhlsp/%E6%BC%AB%E8%B0%88Android-Webview%EF%BC%88%E4%BA%8C%EF%BC%89%EF%BC%9A%E5%9C%BA%E6%99%AF%E4%B8%8E%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95](http://blog.leanote.com/post/zbyzhlsp/漫谈Android-Webview（二）：场景与基本用法)

https://www.zybuluo.com/zyl06/note/737188

https://juejin.im/post/5b94ca52e51d450e7d097f38



**2、缓存模式(5种)**

**LOAD_CACHE_ONLY:**  不使用网络，只读取本地缓存数据

**LOAD_DEFAULT:**  根据cache-control决定是否从网络上取数据。

**LOAD_CACHE_NORMAL: API level 17**中已经废弃, 从**API level 11**开始作用同LOAD_DEFAULT模式

**LOAD_NO_CACHE:** 不使用缓存，只从网络获取数据.

**LOAD_CACHE_ELSE_NETWORK**，只要本地有，无论是否过期，或者no-cache，都使用缓存中的数据。
如：www.taobao.com的cache-control为no-cache，在模式LOAD_DEFAULT下，无论如何都会从网络上取数据，如果没有网络，就会出现错误页面；在LOAD_CACHE_ELSE_NETWORK模式下，无论是否有网络，只要本地有缓存，都使用缓存。本地没有缓存时才从网络上获取。
www.360.com.cn的cache-control为max-age=60，在两种模式下都使用本地缓存数据。

https://juejin.im/entry/5ad70987f265da239148a614



zip离线缓存

https://mp.weixin.qq.com/s/AV2SwFfwwJH7xyrIBJemgw



#### 安全问题 

WebView漏洞的根源在于强制其访问攻击者控制的网页。网页中含有攻击者可以控制的JS,因此可能钓鱼，窃取私有文件，甚至是 RCE,带来比较大的危害。

下面主要是4.4系统以上的机型

##### Webview密码明文存储漏洞

WebView默认开启密码保存功能mWebView.setSavePassword(true),如果未关闭，用户输入密码时，会弹出提示框，询问用户是否保存密码，如果选是，密码会明文保存到 /data/data/com.package.name/databases/webview.db

##### WebView域控制不严格漏洞

setAllowFileAccess(true) : 窃取APP任意目录下的私有文件

setAllowUniversalAccessFromFileURLs : 允许通过file域url中的 javascript访问其他的源。



方案

1. 通过以下设置，防止越权访问，跨域等安全问题

   ```java
   setAllowFileAccess(false)
   
   setAllowFileAccessFromFileURLs(false)
   
   setAllowUniversalAccessFromFileURLs(false)
   ```

2. WebSettings.setSavePassword(false)

   关闭密码保存提醒功能

3. 建议开发者通过以下方式移除该JavaScript接口

   ```javascript
   removeJavascriptInterface("searchBoxJavaBridge_")
   
   removeJavascriptInterface("accessibility")；
   
   removeJavascriptInterface("accessibilityTraversal")
   ```

   

https://zhuanlan.zhihu.com/p/21787366

https://zhuanlan.kanxue.com/article-14155.htm