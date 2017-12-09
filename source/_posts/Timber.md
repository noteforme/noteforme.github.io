---
title: Timber
comments: true
date: 2017-12-09 16:41:05
tags:
categories: ANDROID
---

Timber是JakeWharton　开源的日志系统



## 添加库

`    implementation 'com.jakewharton.timber:timber:4.6.0'
`


##  [MyApplication](http://45.77.222.97:3000/root/NyTimes/src/master/app/src/main/java/com/jonzhou/nytime/MyApplication.java)  如下

**注意 **:  一开始日志打印不了还以为 okhttplogger冲突，BuildConfig需要导入项目的包`import com.jonzhou.nytime.BuildConfig;，而不是Timber或其他的包
`


```

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        if (BuildConfig.DEBUG) {
            Timber.plant(new Timber.DebugTree());
        } else {
            Timber.plant(new CrashReportingTree());
        }
    }

    /**
     * A tree which logs important information for crash reporting.
     */
    private static class CrashReportingTree extends Timber.Tree {
        @Override
        protected void log(int priority, String tag, @NonNull String message, Throwable t) {
            if (priority == Log.VERBOSE || priority == Log.DEBUG) {
                return;
            }

            FakeCrashLibrary.log(priority, tag, message);

            if (t != null) {
                if (priority == Log.ERROR) {
                    FakeCrashLibrary.logError(t);
                } else if (priority == Log.WARN) {
                    FakeCrashLibrary.logWarning(t);
                }
            }
        }
    }
}
```
##  使用
jake的例子

[LintActivity](https://github.com/JakeWharton/timber/blob/master/timber-sample/src/main/java/com/example/timber/ui/LintActivity.java) 

```

    // LogNotTimber
    Log.d("TAG", "msg");
    Log.d("TAG", "msg", new Exception());
    android.util.Log.d("TAG", "msg");
    android.util.Log.d("TAG", "msg", new Exception());
    // StringFormatInTimber
    Timber.w(String.format("%s", getString()));
    Timber.w(format("%s", getString()));
    // ThrowableNotAtBeginning
    Timber.d("%s", new Exception());
    // BinaryOperationInTimber
    String foo = "foo";
    String bar = "bar";
    Timber.d("foo" + "bar");
    Timber.d("foo" + bar);
    Timber.d(foo + "bar");
    Timber.d(foo + bar);
    // TimberArgCount
    Timber.d("%s %s", "arg0");
    Timber.d("%s", "arg0", "arg1");
    Timber.tag("tag").d("%s %s", "arg0");
    Timber.tag("tag").d("%s", "arg0", "arg1");
    // TimberArgTypes
    Timber.d("%d", "arg0");
    Timber.tag("tag").d("%d", "arg0");
    // TimberTagLength
    Timber.tag("abcdefghijklmnopqrstuvwx");
    Timber.tag("abcdefghijklmnopqrstuvw" + "x");
    // TimberExceptionLogging
    Timber.d(new Exception(), new Exception().getMessage());
    Timber.d(new Exception(), "");
    Timber.d(new Exception(), null);
    */
  }

  private String getString() {
    return "foo";
  }
```




