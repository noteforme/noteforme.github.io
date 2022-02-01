---
title: databinding
comments: true
date: 2020-04-01 05:52:42
tags:
categories: Jetpack
---



layout expression of the language

https://codelabs.developers.google.com/codelabs/android-databinding/#0

https://developer.android.com/topic/libraries/data-binding





#### Create first layout expression

1.  when you want to use it in your own projects the first step is to enable the library in the modules that will use it:

   ```
   android {
   ...
       dataBinding {
          enabled true
       }
   }
   ```

2. Convert to data binding layout

   打开布局文件，选中根布局的 **ViewGroup**，按住 **option + 回车键**，点击 “**Convert to data binding layout**”，就可以生成 DataBinding 需要的布局规则

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="name"  //不能用下划线 "_"
            type="String"/>

        <variable
            name="lastName"
            type="String"/>
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/plain_name"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginTop="8dp"
            android:layout_marginEnd="128dp"
            android:text="@{name}"			
            android:textAppearance="@style/TextAppearance.AppCompat.Large"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/name_label"/>

        <TextView
            android:id="@+id/plain_lastname"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginTop="8dp"
            android:layout_marginEnd="128dp"
            android:text="@{lastName}"
            android:textAppearance="@style/TextAppearance.AppCompat.Large"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/lastname_label"/>
            
       </androidx.constraintlayout.widget.ConstraintLayout>    
   </layout>
```



3. Bind TextView

```
class PlainOldActivitySolution2 : AppCompatActivity() {

    // Obtain ViewModel from ViewModelProviders
    private val viewModel by lazy { ViewModelProviders.of(this).get(SimpleViewModel::class.java) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val binding: PlainActivitySolution2Binding =
            DataBindingUtil.setContentView(this, R.layout.plain_activity_solution_2)

        binding.name = "John"
        binding.lastName = "lice"

        // TODO: Explicitly setting initial values is a bad pattern. We'll fix that later on.
        updateLikes()
    }
}
}
```



#### Dealing with user events  Observering data

```
   <data>
        <variable
                name="viewmodel"
                type="com.example.android.databinding.basicsample.data.SimpleViewModel"/>
    </data>
```



```
android:onClick="@{() -> viewmodel.onLike()}"
   
binding.viewmodel = viewModel


```



```
 private val _likes =  MutableLiveData(0)
  fun onLike() {
        _likes.value = (_likes.value ?: 0) + 1
  }
```

```
    <TextView
                android:id="@+id/likes"
                android:text="@{Integer.toString(viewmodel.likes)}"
```





####  Using Binding Adapters

1. One parameter

```
  @BindingAdapter("android:text")
    public static void setText(TextView view, CharSequence text) {
        // Some checks removed for clarity

        view.setText(text);
    }
```

```
   @BindingAdapter("app:hideIfZero")
    fun hideIfZero(view: View, number: Int) {
        view.visibility = if (number == 0) View.GONE else View.VISIBLE
    }
```

```
<ProgressBar
            android:id="@+id/progressBar"
            app:hideIfZero="@{viewmodel.likes}"
```



2. Create a Binding Adapter with multiple parameters   

   ```
    <ProgressBar
                   android:id="@+id/progressBar"
                   app:hideIfZero="@{viewmodel.likes}"
                   app:progressScaled="@{viewmodel.likes}"
                   android:max="@{100}"
   ```





#### oberver class field

https://developer.android.com/topic/libraries/data-binding/observability#observable_fields

https://developer.android.com/topic/libraries/data-binding/observability#observable_objects



##### Using Binding Adapters to create custom attributes



```
@BindingAdapter(value=["app:setSleepData"])
fun setSleepData(viewBodySleep: ViewBodySleep, sleepData: String) {
    viewBodySleep.setSleepData(sleepData)
}
```

```
<com.android.util.view.ViewBodySleep
    android:id="@+id/view_body_sleep"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingLeft="55dp"
    android:paddingRight="48dp"
    app:setSleepData="@{vmBodyFragment.spData}"
    app:layout_constraintTop_toTopOf="parent" />
```



https://developer.android.com/topic/libraries/data-binding/expressions#expression_language

[Work with observable data objects](https://developer.android.com/topic/libraries/data-binding/observability)





* isNull

  ```
  android:text='@{viewModel.patientInfo.remoteCheckReceiveName??""}'
  ```

* visible

  ```kotlin
  @BindingAdapter("app:goneUnless")
  fun goneUnless(view: View, visible: Boolean) {
      view.visibility = if (visible) View.VISIBLE else View.GONE
  }
  ```



[android-databinding](https://github.com/googlecodelabs/android-databinding)

https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652046992&idx=1&sn=b8b4c47537be1227eecd01c1eaee2550&chksm=808ca6d5b7fb2fc32d9ec361c91a2958e51a2db1c6b20370e7ef6493bb5cb20314cda6ab059c&scene=38#wechat_redirect

https://juejin.im/post/5d2be05ff265da1bd605d49a

https://mp.weixin.qq.com/s/4UP-pDs0FK66g1QUQvRN6A



定义xml背景

https://juejin.im/post/5b95c6a0e51d450e664b0aa0



https://www.jianshu.com/p/741103ba2ff1





https://www.cs.usfca.edu/~galles/visualization/RedBlack.html