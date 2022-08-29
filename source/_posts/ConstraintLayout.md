---
title: ConstraintLayout
comments: true
date: 2017-10-15 10:37:39
tags: ConstraintLayout
categories: ANDROID
---

#### introduce

https://developer.android.google.cn/reference/androidx/constraintlayout/widget/ConstraintLayout

https://developer.android.google.cn/training/constraint-layout/index.html

https://juejin.cn/post/6949186887609221133#heading-6



#### Widgets dimension 

widget的尺寸可以通过设置`android:layout_width`和`android:layout_height`的属性值来指定，有如下三种方式：

- 使用具体值
- 使用`WRAP_CONTENT`，这样widget会计算自身的尺寸
- Using `0dp`, which is the equivalent of "`MATCH_CONSTRAINT`" (使用`0dp`，这等于“`MATCH_CONSTRAINT`” )



![img](https://developer.android.google.cn/static/reference/androidx/constraintlayout/widget/resources/images/dimension-match-constraints.png)

##### Group

可以控制一组控件的隐藏和显示

```
  <androidx.constraintlayout.widget.Group
        android:id="@+id/gp_1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="gone"
        app:constraint_referenced_ids="cl_h4,cl_h2" />
```

```
((Group)findViewById(R.id.gp_1)).setVisibility(View.GONE);
```

​	`findViewById(R.id.gp_2).setVisibility(View.INVISIBLE);  doesnt not work`

https://juejin.cn/post/6844903875569254414#heading-17



#####  Guideline使用

https://developer.android.com/reference/android/support/constraint/Guideline.html

```
app:layout_constraintGuide_percent="0.2"
app:layout_constraintGuide_begin="100dp"
```

####   特色属性

##### layout_constraintDimensionRatio

指定高度，宽度随着宽高比自适应

```
<ImageView
    android:id="@+id/iv_img01"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:src="@mipmap/img_1"
    app:layout_constraintDimensionRatio="1:1"
    app:layout_constraintLeft_toLeftOf="parent" />
```

<!--指定高度，宽度随着宽高比自适应 app:layout_constraintDimensionRatio="H,16:9"  H: 表示高宽比-->     

layout_constraintHorizontal_bias  先设置约束



#### **Bias**

上面的水平居中,是使用的与父亲左侧对齐+与父亲右侧对齐.  可以理解为左右的有一种约束力,默认情况下,左右的力度是一样大的,那么view就居中了.

当左侧的力度大一些时,view就会偏向左侧.就像下面这样.



> layout_constraintHorizontal_bias 水平约束力
> layout_constraintVertical_bias 垂直约束力



![](https://developer.android.google.cn/static/reference/androidx/constraintlayout/widget/resources/images/centering-positioning-bias.png)

```xml
<android.support.constraint.ConstraintLayout
    <Button
        android:text="按钮1"
        app:layout_constraintHorizontal_bias="0.3"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>
</android.support.constraint.ConstraintLayout>    
```

上面代码就偏左约束



##### WRAP_CONTENT: 强制约束

我们可能希望使用`WRAP_CONTENT`，同时仍然强制执行约束以限制最终的尺寸

- `app:layout_constrainedWidth=”true|false”`



button1居中显示，button2约束在button1的右边、parent的左边：

```xml
<Button
    android:id="@+id/button1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="aaaaaaa"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintBottom_toBottomOf="parent"/>

<Button
    android:id="@+id/button2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="22222222222222222222222222222"
    app:layout_constrainedWidth="true"
    android:layout_marginTop="40dp"
    app:layout_constraintTop_toTopOf="@id/button1"
    app:layout_constraintStart_toEndOf="@id/button1"
    app:layout_constraintEnd_toEndOf="parent"/>

```



对button2使不使用`app:layout_constrainedWidth="true"`的效果如下：

![constraintlayout-constrainted-width-before](ConstraintLayout/constraintlayout-constrainted-width-before.png)

![constraintlayout-constrainted-width-after](ConstraintLayout/constraintlayout-constrainted-width-after.png)



约束width前 & 约束width后



##### 占用父布局的比例



####  chain

 3个Button 两两依赖,相当于组成了一个链

Button均分 width = match_constraint (0dp)

 ![图](ConstraintLayout/ConstraintLayout_000.png)


```
 <Button
        android:id="@+id/bt_00"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Button_00"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toLeftOf="@+id/bt_01" />

    <Button
        android:id="@+id/bt_01"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Button_01"
        app:layout_constraintLeft_toRightOf="@+id/bt_00"
        app:layout_constraintRight_toLeftOf="@+id/bt_02" />

    <Button
        android:id="@+id/bt_02"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Button_02"
        app:layout_constraintLeft_toRightOf="@+id/bt_01"
        app:layout_constraintRight_toRightOf="parent" />
```



app:layout_constraintHorizontal_weight="2"`　

看属性也可以猜到是干嘛用的了.
![宽度为wrap_content](ConstraintLayout/ConstraintLayout_001.png)

```
 <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        app:layout_constraintTop_toBottomOf="@+id/guideline_00"
        android:layout_height="wrap_content">
        <Button
            android:id="@+id/bt_00"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Button_00"
            app:layout_constraintHorizontal_weight="2"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toLeftOf="@+id/bt_01" />

        <Button
            android:id="@+id/bt_01"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Button_01"
            app:layout_constraintHorizontal_weight="1"
            app:layout_constraintLeft_toRightOf="@+id/bt_00"
            app:layout_constraintRight_toLeftOf="@+id/bt_02" />

        <Button
            android:id="@+id/bt_02"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            app:layout_constraintHorizontal_weight="1"
            android:text="Button_02"
            app:layout_constraintLeft_toRightOf="@+id/bt_01"
            app:layout_constraintRight_toRightOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>
```





Button均份　宽度不为match_constraint

![宽度为wrap_content](ConstraintLayout/ConstraintLayout_10.png)


```
 <Button
            android:id="@+id/bt_10"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button_00"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toLeftOf="@+id/bt_11" />

        <Button
            android:id="@+id/bt_11"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button_01"
            app:layout_constraintLeft_toRightOf="@+id/bt_10"
            app:layout_constraintRight_toLeftOf="@+id/bt_12" />

        <Button
            android:id="@+id/bt_12"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button_02"
            app:layout_constraintLeft_toRightOf="@+id/bt_11"
            app:layout_constraintRight_toRightOf="parent" />
```





#### chainStyle

Chains are controlled by attributes set on the first element of the chain (the "head" of the chain):

<img src="https://developer.android.google.cn/static/reference/androidx/constraintlayout/widget/resources/images/chains-head.png" alt="img" style="zoom:50%;" />

The head is the left-most widget for horizontal chains, and the top-most widget for vertical chains.第一个元素决定链style

而且只有放在头节点才有效，否则都没用



![](https://developer.android.google.cn/static/reference/androidx/constraintlayout/widget/resources/images/chains-styles.png)



##### spread Chain

layout_constraintHorizontal_chainStyle  这个属性默认是spread

元素展开

![20220820163354](ConstraintLayout/20220820163354.jpg)



mineutil

activity_constraint.xml

```xml
<Button
    android:id="@+id/bt_00"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:text="Button_00"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toLeftOf="@+id/bt_01" />

<Button
    android:id="@+id/bt_01"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:text="Button_01"
    app:layout_constraintLeft_toRightOf="@+id/bt_00"
    app:layout_constraintRight_toLeftOf="@+id/bt_02" />

<Button
    android:id="@+id/bt_02"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:text="Button_02"
    app:layout_constraintLeft_toRightOf="@+id/bt_01"
    app:layout_constraintRight_toRightOf="parent" />
```



##### Spread inside chain

 endpoints of the chain will not be spread out (两端不会展开)

其实就相当于两端的约束去掉了



![20220820182406](ConstraintLayout/20220820182406.jpg)

```xml
        <Button
            android:id="@+id/bt_10"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Inside"
            android:background="@android:color/holo_blue_light"
            app:layout_constraintHorizontal_chainStyle="spread_inside"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toLeftOf="@+id/bt_11" />

        <Button
            android:id="@+id/bt_11"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Inside"
            android:background="@android:color/holo_blue_light"
            app:layout_constraintLeft_toRightOf="@+id/bt_10"
            app:layout_constraintRight_toLeftOf="@+id/bt_12" />

        <Button
            android:id="@+id/bt_12"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Inside"
            android:background="@android:color/holo_blue_light"
            app:layout_constraintLeft_toRightOf="@+id/bt_11"
            app:layout_constraintRight_toRightOf="parent" />
```



##### Weighted chains

看上面官方的图就能理解了



##### packed chains



![20220820182631](ConstraintLayout/20220820182631.jpg)


```
        <Button
            android:id="@+id/bt_20"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@android:color/holo_blue_light"
            android:text="packed"
            app:layout_constraintHorizontal_chainStyle="packed" //第一个有效
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toLeftOf="@+id/bt_21" />

        <Button
            android:id="@+id/bt_21"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@android:color/holo_blue_light"
            android:text="packed"
            app:layout_constraintLeft_toRightOf="@+id/bt_20"
            app:layout_constraintRight_toLeftOf="@+id/bt_22" />

        <Button
            android:id="@+id/bt_22"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@android:color/holo_blue_light"
            android:text="packed"
            app:layout_constraintLeft_toRightOf="@+id/bt_21"
            app:layout_constraintRight_toRightOf="parent" /> 
```



##### Packed Chain with Bias



![20220820184349](ConstraintLayout/20220820184349.jpg)



```xml
<Button
    android:id="@+id/bt_30"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="@android:color/holo_blue_light"
    android:text="packedBias"
    app:layout_constraintHorizontal_bias="0.2"
    app:layout_constraintHorizontal_chainStyle="packed"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toLeftOf="@+id/bt_31" />

<Button
    android:id="@+id/bt_31"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="@android:color/holo_blue_light"
    android:text="packedBias"
    app:layout_constraintLeft_toRightOf="@+id/bt_30"
    app:layout_constraintRight_toLeftOf="@+id/bt_32" />

<Button
    android:id="@+id/bt_32"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="@android:color/holo_blue_light"
    android:text="packedBias"
    app:layout_constraintLeft_toRightOf="@+id/bt_31"
    app:layout_constraintRight_toRightOf="parent" />
```







#### Barrier 

用于控制 Barrier 相对于给定的 View 的位置， app:barrierDirection="right",表示barrier在constraint_referenced_ids给定ID的右侧,constraint_referenced_ids的id就是左侧的这些控件

输入信息挺有用的 

https://mp.weixin.qq.com/s/QIuww9b0TsNjajEUS8c2fg

```
  <android.support.constraint.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="@dimen/dimen_11">

        <TextView
            android:id="@+id/tv_order_num"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="订单编号：10002220000001234"
            android:textColor="#ff999999"
            android:textSize="19sp"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent" />

        <TextView
            android:id="@+id/tv_order_pro"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="@dimen/dimen_11"
            android:text="订购产品：爱达康高血压监护服务"
            android:textColor="#ff666666"
            android:textSize="21sp"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/tv_order_num" />

        <TextView
            android:id="@+id/tv_service_team"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="服务团队：爱达康远程监护团队"
            android:textColor="#ff666666"
            android:textSize="21sp"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/tv_order_pro" />

        <TextView
            android:id="@+id/tv_period_valid"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="有效期：2019-03-01至2019-12-31"
            android:textColor="#ff999999"
            android:textSize="19sp"
            app:layout_constraintStart_toEndOf="@+id/barrier" />

        <TextView
            android:id="@+id/tv_order_time"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="@dimen/dimen_11"
            android:text="订购时间：2019-03-11 12:00:00"
            android:textColor="#ff666666"
            android:textSize="21sp"
            app:layout_constraintStart_toEndOf="@+id/barrier"
            app:layout_constraintTop_toBottomOf="@+id/tv_period_valid" />

        <TextView
            android:id="@+id/tv_order_count"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="订购数量：500次"
            android:textColor="#ff666666"
            android:textSize="21sp"
            app:layout_constraintStart_toEndOf="@+id/barrier"
            app:layout_constraintTop_toBottomOf="@+id/tv_order_time" />


        <android.support.constraint.Barrier
            android:id="@+id/barrier"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:barrierDirection="right"
            app:constraint_referenced_ids="tv_order_num,tv_order_pro,tv_service_team" />

    </android.support.constraint.ConstraintLayout>
```



#### Dimensions constraints 

You can define minimum and maximum sizes for the `ConstraintLayout` itself:

- `android:minWidth` set the minimum width for the layout
- `android:minHeight` set the minimum height for the layout

可以设置百分比



#### Practice

需求是美女跟在文字后面，如果文字太长就用省略号

<img src="ConstraintLayout/20220821165517.jpg" alt="20220821165517" style="zoom: 67%;" />



![20220821165912](ConstraintLayout/20220821165912.jpg)



主要着两个属性



```xml
app:layout_constrainedWidth="true" // 如果不用这个约束，文字太长,会把直接在Learn more后面
app:layout_constraintHorizontal_bias="0" // 去掉左边的约束
app:layout_constraintHorizontal_chainStyle="packed" // 让文字和美女在一起
```



```xml
<ImageView
    android:id="@+id/iv_duitnow"
    android:layout_width="36dp"
    android:layout_height="36dp"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent" />

<TextView
    android:id="@+id/tv_duit_text"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_marginStart="9dp"
    android:textColor="#282828"
    android:textSize="22sp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toEndOf="@id/iv_duitnow"
    app:layout_constraintTop_toTopOf="parent"
    android:text="Reload via  Transfer:" />

<TextView
    android:id="@+id/tv_duitnow_id"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginTop="4dp"
    android:ellipsize="end"
    android:lines="1"
    android:textColor="#0064FF"
    android:textSize="14sp"
    app:layout_constrainedWidth="true"
    app:layout_constraintEnd_toStartOf="@+id/img_copy"
    app:layout_constraintHorizontal_bias="0"
    app:layout_constraintHorizontal_chainStyle="packed"
    app:layout_constraintStart_toStartOf="@+id/tv_duit_text"
    app:layout_constraintTop_toBottomOf="@+id/tv_duit_text"
    android:text="0124455660124455660124455666" />

<ImageView
    android:id="@+id/img_copy"
    android:layout_width="30dp"
    android:layout_height="30dp"
    android:scaleType="fitXY"
    android:src="@drawable/meitu111e33"
    app:layout_constraintBottom_toBottomOf="@+id/tv_duitnow_id"
    app:layout_constraintEnd_toStartOf="@+id/bt_learn_more"
    app:layout_constraintStart_toEndOf="@+id/tv_duitnow_id"
    app:layout_constraintTop_toTopOf="@+id/tv_duitnow_id" />


<TextView
    android:id="@+id/bt_learn_more"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:paddingLeft="15dp"
    android:paddingTop="7dp"
    android:background="@android:color/holo_blue_light"
    android:paddingRight="15dp"
    android:paddingBottom="7dp"
    android:textSize="10sp"
    android:textStyle="normal"
    app:layout_constraintBottom_toBottomOf="@+id/tv_duitnow_id"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintTop_toTopOf="@+id/tv_duitnow_id"
    android:text="Learn_More" />
```







https://blog.yorek.xyz/android/other/constraintlayout/#63-wrap_content-11

https://juejin.cn/post/6844903733948579847

https://jishuin.proginn.com/p/763bfbd5c73f

