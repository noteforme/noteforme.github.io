---
title: LOG
comments: true
date: 2017-12-09 16:41:05
tags:
categories: ANDROID
---



##### Timber

1. 添加库

`    implementation 'com.jakewharton.timber:timber:4.6.0'
`



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
2. 使用

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



##### Xlog

1 . 环境配置 

 安装 pyelliptic1.5.7  ` sudo python setup.py install`



2. 解码日志 

 `python mars/log/crypt/decode_mars_nocrypt_log_file.py  MarsSample_20200417.xlog`

但是现在碰到了问题

mq初始化有日志，接收消息没有日志，可能在不同的线程里，需要做处理




