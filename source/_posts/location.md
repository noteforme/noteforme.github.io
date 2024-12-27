---
title: location
date: 2022-05-24 14:48:58
tags:
---

# Location Permission changes

## Background location

The background location precision is the same as the foreground location precision, which depends on the location permissions that your app declares.

On Android 10 (API level 29) and higher, you must declare the ACCESS_BACKGROUND_LOCATION permission in your app's manifest in order to request background location access at runtime. On earlier versions of Android, when your app receives foreground location access, it automatically receives background location access as well.

```
<manifest ... >
  <!-- Required only when requesting background location access on
       Android 10 (API level 29) and higher. -->
  <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
</manifest>
```


https://www.youtube.com/watch?v=xTVeFJZQ28c

https://github.com/baurine/android-location-study/blob/master/note/location-note.md

https://blog.csdn.net/dxiaolai/article/details/81588921

https://blog.csdn.net/qq_32770809/article/details/115914021

https://blog.csdn.net/u013334392/article/details/91971758

其实在上面我已经提到了，所有上面的解决的方案都没有解决根本问题，那就是当你在室内开发时，你的手机根本就没法获取位置信息，你叫系统如何将位置信息通知给你的程序。所以要从根本上解决这个问题，就要解决位置信息获取问题。刚刚也提到了，只有NETWORK_PROVIDER这种模式才是室内定位可靠的方式，只不过由于大陆的怪怪网络，且大部分厂商也不会用google的服务，这种定位方式默认是没法用的。那怎么办？好办，找个替代的服务商就可以了，百度的位置信息sdk就可以解决这个问题。它的基本原理在上面已经提到过了，就是搜集你的wifi节点信息和你的手机基站信息来定位。

#detect installed app

https://stackoverflow.com/questions/72123141/how-to-check-exists-app-in-device-on-android

https://medium.com/@GoogleDroids/how-to-check-if-an-android-app-is-installed-on-your-device-f8f0f1d4fb6a

```
    fun isPackageInstalled(context: Context, packagename: String): Boolean {
        var result = false
        try {
            // is the application installed?
            context.packageManager.getPackageInfo(packagename, PackageManager.GET_ACTIVITIES)
            result = true
        } catch (e: PackageManager.NameNotFoundException) {
            //Not installed
        }
        return result
    }

            val checkInstallation = isPackageInstalled(this, "com.google.android.apps.maps")
            Log.i(TAG, "googleMap: $checkInstallation")

            Log.i(TAG, "gaodemap: ${isPackageInstalled(this, "com.autonavi.minimap")}") 
            Log.i(TAG, "baidumap: ${isPackageInstalled(this, "com.baidu.BaiduMap")}")
            Log.i(TAG, "waze: ${isPackageInstalled(this, "com.waze")}")
```

Androidmanifest.xml

```
    <queries>
        <package android:name="com.google.android.apps.maps" />
        <package android:name="com.google.android.youtube" />
<!--
        <package android:name="com.autonavi.minimap" />

        <package android:name="com.baidu.BaiduMap" />
        -->
        <package android:name="com.waze" />
    </queries>
```

it also can be used , but it seems not recommend in office

```
    <uses-permission android:name="android.permission.QUERY_ALL_PACKAGES"
        tools:ignore="QueryAllPackagesPermission" />

    <queries>
        <intent>
            <action android:name="android.intent.action.MAIN" />
        </intent>
    </queries>
```

huawei OS install others map , except google map
result:

googleMap: false
gaodemap: false
baidumap: false
waze: true

if remove Androidmanifest comment 

googleMap: false
gaodemap: true
baidumap: true
waze: true


##  Accuracy

### Approximate
Provides a device location estimate. If this location estimate is from the LocationManagerService or FusedLocationProvider, this estimate is accurate to within about 3 square kilometers (about 1.2 square miles). Your app can receive locations at this level of accuracy when you declare the ACCESS_COARSE_LOCATION permission but not the ACCESS_FINE_LOCATION permission.
### Precise
Provides a device location estimate that is as accurate as possible. If the location estimate is from LocationManagerService or FusedLocationProvider, this estimate is usually within about 50 meters (160 feet) and is sometimes as accurate as within a few meters (10 feet) or better. Your app can receive locations at this level of accuracy when you declare the ACCESS_FINE_LOCATION permission.


# Google play Policy

https://support.google.com/googleplay/android-developer/answer/10144311#zippy=%2Cexamples-of-common-violations

An app that accesses a user's inventory of installed apps and doesn't treat this data as personal or sensitive data subject to the above Privacy Policy, data handling, and Prominent Disclosure and Consent requirements.

Android 11 introduced changes related to package visibility. These changes affect applications, only if they target Android 11 and above. For more information on these changes, please view the official documentation about package visibility on Android.

https://developer.android.com/training/package-visibility

https://developer.android.com/about/versions/11/privacy/package-visibility

But be aware that Play Store has new policy and common apps with this perm declared won't be pulished in there. You have to provide good reason for accepting your app - this permission must be used for starting user-initiated key feature of app

https://stackoverflow.com/questions/72123141/how-to-check-exists-app-in-device-on-android


# lanuch Google Map
Maps URLs (recommended)
Using Maps URLs, you can build a universal, cross-platform URL to launch Google Maps and perform searches, get directions and navigation, and display map views and panoramic images. These universal URLs allow for broader handling of the maps requests no matter which platform the user is on.

Google Maps Intents for Android
Using intents in your Android app, you can start an activity in another app by describing a simple action you'd like to perform (such as "display a map" or "show directions to the airport") in an Intent object. The Google Maps app for Android supports several different intents, allowing you to launch the Google Maps app in display, search, navigation, or Street View modes.

https://developers.google.com/maps/documentation/android-sdk/intents
