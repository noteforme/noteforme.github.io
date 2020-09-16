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





##### json and serializtion

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

https://www.jianshu.com/p/f37d8546ab8f



*  修改包名后 

  >  target file "lib/main_dev.dart" not found

```
plugin:
  platforms:
    android:
      package: com.casanube.flutter_ble //记得修改这个包名 要不然爆出上面的错误
      pluginClass: FlutterBlePlugin
```