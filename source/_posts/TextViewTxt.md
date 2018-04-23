---
title: TextViewTxt
comments: true
date: 2018-03-04 21:16:56
tags: 
categories: ANDROID
---

TextView使用
https://juejin.im/entry/5729d28f1ea49300606854c9
SpannableString有一个不方便的地方是截取字符串


	    TextView tvRegister = findViewById(R.id.tv_register);
	    //将TextView的显示文字设置为SpannableString
	    tvRegister.setText(getClickableSpan());
	    //设置该句使文本的超连接起作用
	    tvRegister.setMovementMethod(LinkMovementMethod.getInstance());
	    
	   private SpannableString getClickableSpan() {
	    SpannableString spanTxt = new SpannableString("阅读并同意<<用户注册协议>>");
	    //设置文字的前景色
	    spanTxt.setSpan(new ForegroundColorSpan(Color.GREEN), 5, 15, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
	    spanTxt.setSpan(new ClickableSpan() {
	        @Override
	        public void onClick(View widget) {
	            Intent intent = new Intent(TextViewActivity.this, FirstActivity.class);
	            startActivity(intent);
	        }
	    }, 5, 15, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
	    return spanTxt;
	 }

##  EditText失去焦点

 style里面的配置

```
	    <item name="android:focusable">true</item>
        <item name="android:focusableInTouchMode">true</item>
```



https://blog.csdn.net/u011630575/article/details/50775639