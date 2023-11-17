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




verify the notfication  enable

Android  13
granted permission and channel is null

granted permission and channel open



