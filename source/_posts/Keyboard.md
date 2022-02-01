---
title: Keyboard
comments: true
date: 2017-12-21 14:49:56
tags: keyboard
categories: ANDROID
---



https://developer.android.com/training/keyboard-input/style.html




```java
	  static Timer timer = new Timer();
	
      timer.schedule(new TimerTask() {
            @Override
            public void run() {

                showSoftKeyboard(etInvestMoney);
            }
        }, 200);


	private void showSoftKeyboard(View view) {
        if (view.requestFocus()) {
            InputMethodManager imm = (InputMethodManager) getActivity().getSystemService(Context.INPUT_METHOD_SERVICE);
            imm.showSoftInput(view, InputMethodManager.RESULT_UNCHANGED_SHOWN);
        }
    }


```



http://www.jianshu.com/p/215b388a6e7d

https://juejin.im/post/5eddf8456fb9a04804041738