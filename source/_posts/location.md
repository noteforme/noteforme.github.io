---
title: location
date: 2022-05-24 14:48:58
tags:
---



https://www.youtube.com/watch?v=xTVeFJZQ28c

https://github.com/baurine/android-location-study/blob/master/note/location-note.md



https://blog.csdn.net/dxiaolai/article/details/81588921

https://blog.csdn.net/qq_32770809/article/details/115914021

https://blog.csdn.net/u013334392/article/details/91971758



其实在上面我已经提到了，所有上面的解决的方案都没有解决根本问题，那就是当你在室内开发时，你的手机根本就没法获取位置信息，你叫系统如何将位置信息通知给你的程序。所以要从根本上解决这个问题，就要解决位置信息获取问题。刚刚也提到了，只有NETWORK_PROVIDER这种模式才是室内定位可靠的方式，只不过由于大陆的怪怪网络，且大部分厂商也不会用google的服务，这种定位方式默认是没法用的。那怎么办？好办，找个替代的服务商就可以了，百度的位置信息sdk就可以解决这个问题。它的基本原理在上面已经提到过了，就是搜集你的wifi节点信息和你的手机基站信息来定位。



### detect installed app

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


### Google play Policy

https://support.google.com/googleplay/android-developer/answer/10144311#zippy=%2Cexamples-of-common-violations

An app that accesses a user's inventory of installed apps and doesn't treat this data as personal or sensitive data subject to the above Privacy Policy, data handling, and Prominent Disclosure and Consent requirements.


Android 11 introduced changes related to package visibility. These changes affect applications, only if they target Android 11 and above. For more information on these changes, please view the official documentation about package visibility on Android.

https://developer.android.com/training/package-visibility

https://developer.android.com/about/versions/11/privacy/package-visibility







