---
title: Accessibility
comments: true
date: 2017-08-09 21:27:05
tags:
categories: ANDROID
---


 先上官网地址  https://developer.android.com/guide/topics/ui/accessibility/services.html
下面的是例子  https://developer.android.com/training/accessibility/index.html

然后是codelab 看看 https://codelabs.developers.google.com/codelabs/developing-android-a11y-service/index.html?index=..%2F..%2Findex#0

这是codelab github上的地址   https://github.com/googlecodelabs/android-accessibility
一、简单实现
1、创建一个   MyAccessibilityService类  继承 AccessibilityService

...

    public class MyAccessibilityService extends AccessibilityService {

     @Override
     public void onAccessibilityEvent(AccessibilityEvent event) {
     }

     @Override
     public void onInterrupt() {
    }
   }
...


2、创建一个values下创建XML文件夹


<accessibility-service
     android:accessibilityEventTypes="typeViewClicked|typeViewFocused"
     android:packageNames="com.example.android.myFirstApp, com.example.android.mySecondApp"
     android:accessibilityFeedbackType="feedbackSpoken"
     android:notificationTimeout="100"
     android:settingsActivity="com.example.android.apis.accessibility.TestBackActivity"
     android:canRetrieveWindowContent="true"
/>


3、 实现 onServiceConnected()


    @Override
    public void onServiceConnected() {
         // Set the type of events that this service wants to listen to.  Others
         // won't be passed to this service.
         info.eventTypes = AccessibilityEvent.TYPE_VIEW_CLICKED |
                AccessibilityEvent.TYPE_VIEW_FOCUSED;

         // If you only want this service to work with specific applications, set their
        // package names here.  Otherwise, when the service is activated, it will listen
        // to events from all applications.
        info.packageNames = new String[]
                {"com.example.android.myFirstApp", "com.example.android.mySecondApp"};

        // Set the type of feedback your service will provide.
        info.feedbackType = AccessibilityServiceInfo.FEEDBACK_SPOKEN;

        // Default services are invoked only if no package-specific ones are present
        // for the type of AccessibilityEvent generated.  This service *is*
        // application-specific, so the flag isn't necessary.  If this was a
        // general-purpose service, it would be worth considering setting the
        // DEFAULT flag.

        // info.flags = AccessibilityServiceInfo.DEFAULT;

         info.notificationTimeout = 100;

         this.setServiceInfo(info);
         Log.i("test","测试");
    }

4、实现 onAccessibilityEvent方法


    @Override
    public void onAccessibilityEvent(AccessibilityEvent event) {
     final int eventType = event.getEventType();
     String eventText = null;
     switch(eventType) {
        case AccessibilityEvent.TYPE_VIEW_CLICKED:
            eventText = "Focused: ";
            break;
        case AccessibilityEvent.TYPE_VIEW_FOCUSED:
            eventText = "Focused: ";
            break;
     }

     eventText = eventText + event.getContentDescription();

     // Do something nifty with this text, like speak the composed string
    // back to the user.
     speakToUser(eventText);
    }
    ...


然后就可以开启辅助功能，看下log

