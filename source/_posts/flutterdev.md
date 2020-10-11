---
title: flutterdev
comments: true
date: 2020-08-26 11:56:46
tags:
categories: flutter
---

​		

![CgqCHl7zAM2AFYCOAAFd30sb1Ck089](flutterdev/CgqCHl7zAM2AFYCOAAFd30sb1Ck089.png)



https://flutter.dev/docs/development/ui/layout

#### Aligning widgets

##### MainAxisAlignment

https://blog.csdn.net/BINGXIHEART/article/details/95077098

 可以接着完善

* row 

  ```dart
  Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Image.asset('images/pic1.jpg'),
      Image.asset('images/pic2.jpg'),
      Image.asset('images/pic3.jpg'),
    ],
  );
  ```

  <img src="flutterdev/Screen Shot 2020-08-26 at 11.55.17 AM.png" alt="Screen Shot 2020-08-26 at 11.55.17 AM" style="zoom: 50%;" />

* Column

  ```
  Widget buildRow() => Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Image.asset('images/pic1.jpg'),
          Image.asset('images/pic2.jpg'),
          Image.asset('images/pic3.jpg'),
        ],
      );
  ```

  <img src="flutterdev/Screen Shot 2020-08-26 at 12.04.23 PM.png" alt="Screen Shot 2020-08-26 at 12.04.23 PM" style="zoom: 33%;" />



####  constraints

##### ConstrainedBox

  The `ConstrainedBox` imposes **additional** constraints from its `constraints` parameter onto its child .

#####  UnconstrainedBox

The screen forces the `UnconstrainedBox` to be exactly the same size as the screen. However, the `UnconstrainedBox` lets its child `Container` be any size it wants





https://resocoder.com/2019/08/15/flutter-custom-icons-automatic-manual-way-icon-font-or-svg/

https://www.fluttericon.com/



#### json and serializtion

```
dependencies:
	json_annotation: ^2.2.0


dev_dependencies:
  flutter_test:
    sdk: flutter

  build_runner: ^1.0.0
  json_serializable: ^2.0.0
  json_model:
```

> flutter packages pub run json_model

```
import 'dart:convert';
import 'dart:io';

import 'package:flutteruse/models/user.dart';

void main() {
  final jsonString = File('jsons/user.json').readAsStringSync();
  print(jsonString);

  // var jsonString = '{"name": "John Smith","email": "john@example.com"}';
  // Map<String, dynamic> user = jsonDecode(jsonString) as Map<String, dynamic>;
  // print('Howdy, ${user['name']}!');
  // print('We sent the verification link to ${user['email']}.');

  Map userMap = jsonDecode(jsonString);
  var user = User.fromJson(userMap);
  print('Howdy, ${user.name}!');
  print('We sent the verification link to ${user.email}.');

  print(user.toJson());
}
```



项目中采用第三种方法

**使用json_model自动生成**

1.在项目根目录新建jsons文件夹并将json数据新建成为文件，在lib目录下新建包名为models

注意：这两个文件夹的名字都必须为jsons和models

![img](https:////upload-images.jianshu.io/upload_images/20009961-2fef1bf9e56b2ad9.png?imageMogr2/auto-orient/strip|imageView2/2/w/658/format/webp)

2、加入依懒

![img](https:////upload-images.jianshu.io/upload_images/20009961-48c442858ef185e1.png?imageMogr2/auto-orient/strip|imageView2/2/w/794/format/webp)

注意：这里添加完json_serializable相关依赖之后还添加了json_model的依赖，这就是快捷生成的关键，还有如果json中引用了其他model可以使用如下方式

{

"name":"wendux",

"father":"$user",//可以通过"$"符号引用其它model类

"friends":"$[]user",//可以通过"$[]"来引用数组

"keywords":"$[]String",//同上

"age":20

}

**3.完成上面的操作之后**

在当前项目的根目录执行如下命令：flutter packages pub run json_model

![img](https:////upload-images.jianshu.io/upload_images/20009961-1aa8170bb500d178.png?imageMogr2/auto-orient/strip|imageView2/2/w/1157/format/webp)

控制台打印如下日志就成功了

![img](https:////upload-images.jianshu.io/upload_images/20009961-e3a705329f02ae39.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

注意：如果json文件中加了注释可能会报错，需要删除注释，错误信息如下

![img](https:////upload-images.jianshu.io/upload_images/20009961-bbf32b1d29e8a279.png?imageMogr2/auto-orient/strip|imageView2/2/w/1182/format/webp)



其实，我们也可以不使用命令行的方式自动生成，

![img](https:////upload-images.jianshu.io/upload_images/20009961-f2f6d6dfb8e1f5e8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1058/format/webp)

用run(['src=jsons'])的方法也可以，run方法为json_model暴露的方法



https://www.jianshu.com/p/f37d8546ab8f



*  修改包名后  pubspec.yaml

  >  target file "lib/main_dev.dart" not found

```
plugin:
  platforms:
    android:
      package: com.casanube.flutter_ble //记得修改这个包名 要不然爆出上面的错误
      pluginClass: FlutterBlePlugin
```



flutter自定义控件

https://mp.weixin.qq.com/s/aVvos5iWYVmiVZRv4hMQMQ

https://juejin.im/post/6874061694491721736