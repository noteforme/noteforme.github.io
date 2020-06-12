---
title: room
comments: true
date: 2019-09-20 10:22:52
tags: DB
categories: ANDROID
---

https://developer.android.com/training/data-storage/room



##### Room

 https://codelabs.developers.google.com/codelabs/android-room-with-a-view/#0

https://codelabs.developers.google.com/codelabs/android-room-with-a-view-kotlin/#0

kotlin need familiar [basic coroutines](

#####  数据库基本操作

* 主键

  每个实体必须定义至少1个字段作为主键。即使只有1个字段，仍然需要用`@PrimaryKey`注解字段。此外，如果您想Room自动分配IDs给实体，则可以设置`@PrimaryKey`的`autoGenerate`属性。如果实体具有复合主键，则可以使用`@Entity`注解的`primaryKeys`属性



* Entity

  ```
  @Entity
  public class BleData {
      @PrimaryKey
      public int id;
  
      public long createDttm;
      public String bleParam;
  }
  ```

* Dao

  
  
  

https://developer.android.com/topic/libraries/architecture/room

https://github.com/googlecodelabs/android-room-with-a-view

https://codelabs.developers.google.com/codelabs/android-room-with-a-view/#0

https://github.com/googlecodelabs/android-room-with-a-view/tree/kotlin



https://blog.skymxc.com/2018/04/15/Room/

https://www.jianshu.com/p/0ed8b17a199e

索引为什么加快访问速度

####  数据库迁移

https://www.jianshu.com/p/72eeaded8913

https://blog.csdn.net/a254837127/article/details/84564545

https://developer.android.com/training/data-storage/room#java



https://codelabs.developers.google.com/codelabs/kotlin-coroutines/#0)

