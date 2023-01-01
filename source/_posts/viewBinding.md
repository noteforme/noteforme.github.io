---
title: viewBinding
date: 2022-12-27 21:04:33
tags: ANDROID
---



https://www.cnblogs.com/pengxurui/p/16669380.html



https://juejin.cn/post/6958346113552220173



##### 编译生成的目录

build/generated/data_binding_base_class_source_out/



ActivityCBinding.java

```java
public final class ActivityCBinding implements ViewBinding {
  @NonNull
  private final LinearLayout rootView;

  @NonNull
  public final Button btC;

  @NonNull
  public final Button btD;

  private ActivityCBinding(@NonNull LinearLayout rootView, @NonNull Button btC,
      @NonNull Button btD) {
    this.rootView = rootView;
    this.btC = btC;
    this.btD = btD;
  }

  @Override
  @NonNull
  public LinearLayout getRoot() {
    return rootView;
  }

  @NonNull
  public static ActivityCBinding inflate(@NonNull LayoutInflater inflater) {
    return inflate(inflater, null, false);
  }

  @NonNull
  public static ActivityCBinding inflate(@NonNull LayoutInflater inflater,
      @Nullable ViewGroup parent, boolean attachToParent) {
    View root = inflater.inflate(R.layout.activity_c, parent, false);
    if (attachToParent) {
      parent.addView(root);
    }
    return bind(root);
  }

  @NonNull
  public static ActivityCBinding bind(@NonNull View rootView) {
    // The body of this method is generated in a way you would not otherwise write.
    // This is done to optimize the compiled bytecode for size and performance.
    int id;
    missingId: {
      id = R.id.bt_c;
      Button btC = ViewBindings.findChildViewById(rootView, id);
      if (btC == null) {
        break missingId;
      }

      id = R.id.bt_D;
      Button btD = ViewBindings.findChildViewById(rootView, id);
      if (btD == null) {
        break missingId;
      }

      return new ActivityCBinding((LinearLayout) rootView, btC, btD);
    }
    String missingId = rootView.getResources().getResourceName(id);
    throw new NullPointerException("Missing required view with ID: ".concat(missingId));
  }
}
```



从控件  Button btC;  Button btD;可以很容易看到。id都是findbyid生成的。





```kotlin
class FragmentViewBinding<T : ViewBinding>(classes: Class<T>, fragment: Fragment) :
    FragmentDelegate<T>(fragment) {

    private val TAG = "TAG"

    private val layoutInflater = classes.inflateMethod()

    private val bindView = classes.bindMethod()

    //调用getValue属于属性代理,访问viewbiding属性调用getVaule方法
    @Suppress("UNCHECKED_CAST")
    override fun getValue(thisRef: Fragment, property: KProperty<*>): T = viewBinding ?: let {

        Log.i(TAG, "getValue: $layoutInflater  $bindView")

        val bind: T = (if (thisRef.view == null) {
            layoutInflater.invoke(null, thisRef.layoutInflater) // 表示 ActivityCBinding.inflate()
        } else {
            bindView.invoke(null, thisRef.view) //表示 ActivityCBinding.bind()
        }) as T
        viewBinding = bind
        bind
    }
}
```
