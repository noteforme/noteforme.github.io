---
title: viewmodel
comments: true
date: 2020-04-06 15:51:24
tags:
categories: Jetpack
---

https://developer.android.com/topic/libraries/architecture/viewmodel



<<<<<<< HEAD
##### Share data between fragments
=======
#### livedata和viewmodel关系

viewmodel中的数据发生变化时通知页面



#### ViewModel出现前

1. 瞬间数据丢失
2. 异步调用的内存泄漏
3. 类膨胀提高维护难度和测试难度





![](viewmodel/2021-08-02_8.18_viewmodel.png)

Share data between fragments
>>>>>>> d7906d4909f312e25743be4a4c56ad1539df1ea6

It's very common that two or more fragments in an activity need to communicate with each other

```
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



<<<<<<< HEAD
##### 使用

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
=======


#### ViewModel保持数据原理

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
>>>>>>> d7906d4909f312e25743be4a4c56ad1539df1ea6
