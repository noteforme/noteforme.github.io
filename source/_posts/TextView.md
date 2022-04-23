---
title: TextView
comments: true
date: 2018-03-04 21:16:56
tags: 
categories: ANDROID
---

 

##### TextView颜色变化

SpannableString

https://juejin.im/entry/5729d28f1ea49300606854c9
SpannableString有一个不方便的地方是截取字符串


```java
    TextView tvRegister = findViewById(R.id.tv_register);
      //将TextView的显示文字设置为SpannableString
​	    tvRegister.setText(getClickableSpan());
​	    //设置该句使文本的超连接起作用
​	    tvRegister.setMovementMethod(LinkMovementMethod.getInstance());
​	    
​	   private SpannableString getClickableSpan() {
​	    SpannableString spanTxt = new SpannableString("阅读并同意<<用户注册协议>>");
​	    //设置文字的前景色
​	    spanTxt.setSpan(new ForegroundColorSpan(Color.GREEN),5, 15,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
​	    spanTxt.setSpan(new ClickableSpan() {	
​	        @Override
​	        public void onClick(View widget) {
​	            Intent intent = new Intent(TextViewActivity.this, FirstActivity.class);
​	            startActivity(intent);
​	        }
​	    }, 5, 15, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
​	  
```


​	

Html标签方式



```java
	TextView textView = (TextView) findViewById(R.id.text_for_test);
	String textSource = "修改TextView中部分文字的<font color='#ff0000'><big>大</big><small>小</small></font>和<font color='#00ff00'>颜色</font>，展示多彩效果！";
	textView.setText(Html.fromHtml(textSource));
```





http://www.jianshu.com/p/f6cef78e8652

https://github.com/liqy/TextViewDemo



#####  EditText

style里面的配置

```
	     <item name="android:focusable">true</item>
       <item name="android:focusableInTouchMode">true</item>
```

 https://blog.csdn.net/u011630575/article/details/50775639

下划线颜色

https://blog.csdn.net/lindroid20/article/details/72551102

自定义解析器

https://my.oschina.net/ososchina/blog/3018393

密码输入框

https://github.com/li504799868/ZEditText



##### 超出字符显示省略号

https://www.jianshu.com/p/025f49696882



监听器设置



https://blog.csdn.net/xiao_ziqiang/article/details/50790168

https://blog.csdn.net/u014748504/article/details/79083501

https://www.jianshu.com/p/1d0777d9bfdf

