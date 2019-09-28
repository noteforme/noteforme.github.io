---
title: GreenDao
comments: true
date: 2019-08-12 17:42:58
tags: db
categories: ANDROID
---

#####  数据库定义

* Bean

>  schema：告知GreenDao当前实体属于哪个schema
>  active：标记一个实体处于活跃状态，活动实体有更新、删除和刷新方法
>  nameInDb：在数据库中使用的别名，默认使用的是实体的类名
>  indexes：定义索引，可以跨越多个列
>  createInDb：标记创建数据库表

* 基础属性注解

  >@Id：主键 Long 型，可以通过@Id(autoincrement = true)设置自增长
  >@Property：设置一个非默认关系映射所对应的列名，默认是使用字段名，例如：@Property(nameInDb = "name")
  > @NotNull：设置数据库表当前列不能为空
  > @Transient：添加此标记后不会生成数据库表的列
  >
  >

* 索引注解

> @Index：使用@Index作为一个属性来创建一个索引，通过name设置索引别名，也可以通过unique给索引添加约束
> @Unique：向数据库添加了一个唯一的约束

- 关系注解

  >@ToOne：定义与另一个实体（一个实体对象）的关系
  >@ToMany：定义与多个实体对象的关系

<https://blog.csdn.net/speedystone/article/details/72769793>



##### @Convert 自定义类型

```
public @interface Convert {
    Class<? extends PropertyConverter> converter();
    Class columnType(); //可以在DB中保留的列的类。这仅限于greenDAO原生支持的所有java类。
}
@Convert(converter = NoteTypeConverter.class, columnType = String.class) //类型转换类，自定义的类型在数据库中存储的类型
private NoteType type; //在保存到数据库时会将自定义的类型 NoteType 通过 NoteTypeConverter 转换为数据库支持的 String 类型。反之亦然
```

C:\Users\john\Desktop\dbsqlite

#####  查看数据库

Device file explorer里面查看

data/data/{$packname}/mine

https://blog.csdn.net/yu75567218/article/details/78904909



* 条件查询

  https://blog.csdn.net/qq_34358104/article/details/69833909

##### 数据缓存

* RxJava实现数据网络数据缓存

  https://blog.csdn.net/qq_35064774/article/details/53449795

* 用户测量数据有网上传

  https://wenku.baidu.com/view/65f16f61998fcc22bdd10d5b.html

  https://my.oschina.net/banxi/blog/57984

  https://www.infoq.cn/article/q-EjPtHOVFaE46XKjhlf

https://github.com/googlesamples/android-architecture-components/