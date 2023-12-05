---
title: notification
date: 2023-11-15 08:30:59
tags: ANDROID
---


### Notification switch
```
   NotificationManagerCompat.from(this).areNotificationsEnabled()
```


### channel switch
```
NotificationManager.IMPORTANCE_NONE == NotificationManagerCompat.from(this).getNotificationChannel(CHANNEL_ID)?.importance
```

https://stackoverflow.com/questions/71159130/custom-android-notification-sound


detect the notfication  enable

1. Android  13 and above
 granted permission and channel is null
 granted permission and channel open


2. Android 8 and above - below Android 13
   notfication switch open
   channel switch open
3. Blow Android 8
   notification switch open
   






