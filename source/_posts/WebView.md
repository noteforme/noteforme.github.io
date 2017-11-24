---
title: WebView
comments: true
date: 2017-09-01 16:07:15
tags:
categories: ANDROID

---

#　加载网页

```
//方式1. 加载一个网页：
  webView.loadUrl("http://www.google.com/");

  //方式2：加载apk包中的html页面
  webView.loadUrl("file:///android_asset/java.html");

```



# Enabling JavaScript

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

# Binding JavaScript code to Android code(JS交互)
When developing a web application that's designed specifically for the WebView in your Android application, you can create interfaces between your JavaScript code and client-side Android code. For example, your JavaScript code can call a method in your Android code to display a Dialog, instead of using JavaScript's alert() function.

To bind a new interface between your JavaScript and Android code, call addJavascriptInterface(), passing it a class instance to bind to your JavaScript and an interface name that your JavaScript can call to access the class.

1. 然后上例子了

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




2.html小例子
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

# localstorge

# 传参给h5
http://www.jianshu.com/p/c20513cad758