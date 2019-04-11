---
title: WebView
comments: true
date: 2017-09-01 16:07:15
tags:
categories: ANDROID

---

####　基本使用

#####  加载资源
-  加载一个网页：
    `webView.loadUrl("http://www.google.com/");`

-  加载apk包中的html页面
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

##  Binding JavaScript code to Android code(JS交互)
When developing a web application that's designed specifically for the WebView in your Android application, you can create interfaces between your JavaScript code and client-side Android code. For example, your JavaScript code can call a method in your Android code to display a Dialog, instead of using JavaScript's alert() function.

To bind a new interface between your JavaScript and Android code, call addJavascriptInterface(), passing it a class instance to bind to your JavaScript and an interface name that your JavaScript can call to access the class.

* office Demo

```
public class WebAppInterface {
    Context mContext;

    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void showToast(String toast) {
        Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
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