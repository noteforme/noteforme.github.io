---
title: EventBus
comments: true
date: 2018-07-11 14:12:08
tags:
categories: ANDROID
---



1. 定义事件类

   ```
   public class MessageEvent {
       private  String message;
   
       public MessageEvent(String message) {
           this.message = message;
       }
   
       public String getMessage() {
           return message;
       }
   }
   ```

   

2.  EventBusActivity

```
@Subscribe(threadMode = ThreadMode.MAIN)
public  void  onMessageEvent(MessageEvent messageEvent){
    btEventRecieve.setText(messageEvent.getMessage());
}

@Override
 public void onStart() {
     super.onStart();
     EventBus.getDefault().register(this);
 }

 @Override
 public void onStop() {
     super.onStop();
     EventBus.getDefault().unregister(this);
 }
```

3. GoalEventActivity 

```
findViewById(R.id.bt_message_send).setOnClickListener(v->{
    new Thread(()->{
        EventBus.getDefault().post(new MessageEvent("我是 GoalEventActivity"));
    }).start();
});
```





#####  类别

在上面的例子中，我们再注解`@Subscribe(threadMode = ThreadMode.MAIN)`中使用了ThreadMode.MAIN这个模式，表示该函数在主线程即UI线程中执行，实际上EventBus总共有四种线程模式，分别是： 

* ThreadMode.MAIN：表示无论事件是在哪个线程发布出来的，该事件订阅方法onEvent都会在UI线程中执行，这个在Android中是非常有用的，因为在Android中只能在UI线程中更新UI，所有在此模式下的方法是不能执行耗时操作的。
* ThreadMode.POSTING：表示事件在哪个线程中发布出来的，事件订阅函数onEvent就会在这个线程中运行，也就是说发布事件和接收事件在同一个线程。使用这个方法时，在onEvent方法中不能执行耗时操作，如果执行耗时操作容易导致事件分发延迟。
* ThreadMode.BACKGROUND：表示如果事件在UI线程中发布出来的，那么订阅函数onEvent就会在子线程中运行，如果事件本来就是在子线程中发布出来的，那么订阅函数直接在该子线程中执行。
* ThreadMode.AYSNC：使用这个模式的订阅函数，那么无论事件在哪个线程发布，都会创建新的子线程来执行订阅函数。

  

 https://github.com/greenrobot/EventBus

https://www.jianshu.com/p/a040955194fc

https://segmentfault.com/a/1190000004279679