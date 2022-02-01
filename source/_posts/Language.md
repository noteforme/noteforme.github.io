---
title: Language
comments: true
date: 2021-11-06 16:06:32
tags:
categories: ANDROID
---



##### 方式

Android < 7.0一下和以上不同, 

1. 7.0以下只要设置一次语言就可以
2. 7.0或以上，必须在BaseActivity设置



##### BaseActivity处理所有的

省事的做法，调用attachBaseContext,一起设置

```kotlin
object LanguageUtil {
    
    @JvmStatic
    fun attachBaseContext(context: Context, language: String?): Context? {
        val resources = context.resources
        val configuration = resources.configuration
        val locale = getLocaleByLanguage(language!!)
        configuration.setLocale(locale)
        return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            configuration.setLocales(LocaleList(locale))
            context.createConfigurationContext(configuration)
        } else {
            //获取想要切换的语言类型
            // updateConfiguration
            val dm = resources.displayMetrics
            resources.updateConfiguration(configuration, dm)
            context
        }
    }

    fun getLocaleByLanguage(language: String): Locale {
        var locale: Locale
        if (language == TNGLanguageEnumType.CN.language) {
            locale = Locale.SIMPLIFIED_CHINESE
        } else if (language == TNGLanguageEnumType.MY.language) {
            locale = Locale.forLanguageTag("ms")
        } else {
            locale = Locale.ENGLISH
        }
        return locale
    }


    @TargetApi(Build.VERSION_CODES.N)
    private fun updateResources(context: Context, language: String): Context {
        val resources = context.resources
        val locale = getLocaleByLanguage(language)
        val configuration = resources.configuration
        configuration.setLocale(locale)
        configuration.setLocales(LocaleList(locale))
        return context.createConfigurationContext(configuration)
    }


    @JvmStatic
    fun setFontScaleLimit(context: Context): Context? {
        val config = Configuration(context.resources.configuration)
        if (config.fontScale > 1.2f || config.fontScale < 1.0f) {
            config.setToDefaults()
            config.fontScale = 1.1f
        }
        return context.createConfigurationContext(config)
    }
}
```



##### 小于7.0设置一次

changeAppLanguage() 调这个方法

```java
public class LanguageUtil {

    private static final String TAG = "LanguageUtil";

    /**
     * @param context
     * @param newLanguage 想要切换的语言类型 比如 "en" ,"zh"
     */
    @SuppressWarnings("deprecation")
    public static void changeAppLanguage(Context context, String newLanguage) {
        if (TextUtils.isEmpty(newLanguage)) {
            return;
        }
        Resources resources = context.getResources();
        Configuration configuration = resources.getConfiguration();
        //获取想要切换的语言类型
        Locale locale = getLocaleByLanguage(newLanguage);
        configuration.setLocale(locale);
        // updateConfiguration
        DisplayMetrics dm = resources.getDisplayMetrics();
        resources.updateConfiguration(configuration, dm);
    }

    public static Locale getLocaleByLanguage(String language) {
        Locale locale = Locale.ENGLISH;
        if (language.equals(LanguageType.CHINESE.getLanguage())) {
            locale = Locale.SIMPLIFIED_CHINESE;
        } else if (language.equals(LanguageType.ENGLISH.getLanguage())) {
            locale = Locale.ENGLISH;
        } else if (language.equals(LanguageType.MY.getLanguage())) {
            locale = Locale.forLanguageTag(language);
        }
        Log.d(TAG, "getLocaleByLanguage: " + locale.getDisplayCountry());
        return locale;
    }

    public static Context attachBaseContext(Context context, String language) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            return updateResources(context, language);
        } else {
            return context;
        }
    }

    @RequiresApi(api = VERSION_CODES.N)
    private static Context updateResources(Context context, String language) {
        Resources resources = context.getResources();
        Locale locale = LanguageUtil.getLocaleByLanguage(language);

        Configuration configuration = resources.getConfiguration();
        configuration.setLocale(locale);
        configuration.setLocales(new LocaleList(locale));
        return context.createConfigurationContext(configuration);
    }

    public static Context updateBaseContext(Context context, String language) {
        Resources resources = context.getResources();
        Configuration configuration = resources.getConfiguration();
        Locale locale = LanguageUtil.getLocaleByLanguage(language);
        configuration.setLocale(locale);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            configuration.setLocales(new LocaleList(locale));
            return context.createConfigurationContext(configuration);
        }else {
            //获取想要切换的语言类型
            // updateConfiguration
            DisplayMetrics dm = resources.getDisplayMetrics();
            resources.updateConfiguration(configuration, dm);
            return context;
        }

    }

}
```



 https://github.com/wisnukurniawan/changelanguage.git

https://www.jianshu.com/p/4f9db19d9383

