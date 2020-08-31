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

