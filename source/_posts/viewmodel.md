---
id: 437
title: viewmodel
date: '2024-06-11T08:50:10+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=437'
permalink: /2024/06/11/viewmodel/
categories:
    - JETPACK
---



问题, 为什么，这种方式，可以获取application对象

 private val myViewModel: MyViewModel by viewModels()

class MyViewModel(application: Application) : AndroidViewModel(application) {

https://developer.android.com/topic/libraries/architecture/viewmodel

Share data between fragments

# livedata和viewmodel关系

viewmodel中的数据发生变化时通知页面

# ViewModel出现前

1. 瞬间数据丢失
2. 异步调用的内存泄漏
3. 类膨胀提高维护难度和测试难度

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/viewmodel/2021-08-02_8.14_viewmodel.png)

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/viewmodel/2021-08-02_8.18_viewmodel.png)

Share data between fragments

It's very common that two or more fragments in an activity need to communicate with each other

```kotlin
class SharedViewModel : ViewModel() {
    val selected = MutableLiveData<Item>()

    fun select(item: Item) {
        selected.value = item
    }
}

class MasterFragment : Fragment() {

    private lateinit var itemSelector: Selector

    // Use the 'by activityViewModels()' Kotlin property delegate
    // from the fragment-ktx artifact
    private val model: SharedViewModel by activityViewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        itemSelector.setOnClickListener { item ->
            // Update the UI
        }
    }
}

class DetailFragment : Fragment() {

    // Use the 'by activityViewModels()' Kotlin property delegate
    // from the fragment-ktx artifact
    private val model: SharedViewModel by activityViewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        model.selected.observe(viewLifecycleOwner, Observer<Item> { item ->
            // Update the UI
        })
    }
}
```

https://codelabs.developers.google.com/codelabs/android-lifecycles/#0

# 使用

```java
public class MyViewModel  extends ViewModel { 
    public MyViewModel(Application application){
        super();
    }

    public int number;

}

public class MyViewModel extends AndroidViewModel { //需要用到application就用AndroidViewModel
    public MyViewModel(Application application) {
        super(application);
    }

    public int number;
}
```

# ViewModel保持数据原理

1. 使用

```java
viewModel = new ViewModelProvider(this, new ViewModelProvider.AndroidViewModelFactory(getApplication())).get(MyViewModel.class);
```

2. owner.getViewModelStore()
   
   ```java
   public ViewModelProvider(@NonNull ViewModelStoreOwner owner, @NonNull Factory factory) {
       this(owner.getViewModelStore(), factory);
   }
   ```

3. ComponentActivity
   
   ```java
   @Override
   public ViewModelStore getViewModelStore() {
       if (getApplication() == null) {
           throw new IllegalStateException("Your activity is not yet attached to the "
                   + "Application instance. You can't request ViewModel before onCreate call.");
       }
       if (mViewModelStore == null) {
           NonConfigurationInstances nc =
                   (NonConfigurationInstances) getLastNonConfigurationInstance(); //拿到上次屏幕的状态
           if (nc != null) {
               // Restore the ViewModelStore from NonConfigurationInstances
               mViewModelStore = nc.viewModelStore; // 获取上次的mViewModelStore，里面保存有HashMap<String, ViewModel> mMap
           }
           if (mViewModelStore == null) {
               mViewModelStore = new ViewModelStore();
           }
       }
       return mViewModelStore;
   }
   ```

所以ViewModel横竖屏切换保存数据的秘诀就是

//拿到上次屏幕的状态,获取上次的mViewModelStore，里面保存有HashMap<String, ViewModel> mMap,接着通过.get(MyViewModel.class);获取ViewModel,如果获取不到就反射newInstance()一个。

https://www.bilibili.com/video/BV1Hh411a7LX?p=6&spm_id_from=pageDriver

# Unit test

https://simplifiedcoding.in/all-courses

https://simplifiedcoding.in/course/android-testing-tutorial-from-junit-to-espresso/start/1

点 DOWNLOAD CODE OF SPENDS TRACKER 跳转后就有下载地址

 spend-tracker

https://www.youtube.com/watch?v=B-dJTFeOAqw

https://www.youtube.com/watch?v=uH6-HNxK32E

# SavedStateHandle

https://www.kodeco.com/5212210-jetpack-saved-state-for-viewmodel-getting-started

https://juejin.cn/post/6907121847024746503

# VisibleForTesting

通过上面的代码看出， 被测试的testPrivate方法的可见性还是被改成Protected。也就是,VisibleForTesting只是一个注释，一个元数据metadata，它并没有进入程序逻辑，也没有被转化成字节码byte code 从而被JVM执行。

笔者猜测可能是Guava 的 程序员犯懒了, 即不愿意在unit test里直接利用Reflection来测试私有方法。也没有把私有方法写入另一个类中。所以设计了VisibleForTesting的注解来提醒其他程序员: 这里为了测试私有方法把私有方法改成了Protected(受保护的)并放宽了访问限制。

可是就JAVA本身而言，只有通过Reflection才能真正测试私有方法。

https://www.cnblogs.com/yanlongpankow/p/6240563.html

https://jefflin1982.medium.com/android-visiblefortesting%E7%9A%84%E7%94%A8%E9%80%94-5a666a17ba95

# viewmodel share

https://github.com/google-developer-training/android-basics-kotlin-cupcake-app/tree/starter