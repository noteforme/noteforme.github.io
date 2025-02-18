---
title: flutter_begin
comments: true
date: 2020-01-04 13:46:38
tags:
categories: flutter
---

#### 环境变量

1. bash_profile

> ~/.bash_profile

```
  export PATH=/Users/m/development/flutter/bin:$PATH
  export ANDROID_HOME="/Users/m/Library/Android/sdk"
  export PATH=${PATH}:${ANDROID_HOME}/tools
  export PATH=${PATH}:${ANDROID_HOME}/platform-tools
  export PUB_HOSTED_URL=https://pub.flutter-io.cn
  export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

2. `~/.zshrc` ，在其中添加：source ~/.bash_profile

Project struct -> Modules -> no sdk 选择

https://mp.weixin.qq.com/s/YfzcvebruRk4LJRTQFVDXA

https://github.com/toly1994328/FlutterUnit

##### build apk

出错解决: rm  /Users/john/development/flutter/bin/cache

>  A problem occurred evaluating root project 'agora_rtm'.
> 
> Could not get unknown property 'kotlin_version' for object of type org.gradle.api.internal.artifacts.dsl.dependencies.DefaultDependencyHandler.

版本配置

> /Users/***/flutter/.pub-cache/hosted/pub.flutter-io.cn/cipher2-xxxx/android/build.gradle' , ext.kotlin_version = 1.3.10

> Users/john/.pub-cache/hosted/pub.flutter-io.cn/agora_rtm-0.9.9/android/build.gradle'

https://blog.csdn.net/weixin_44416513/article/details/90298303

#### Widget

##### Text

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Text widget',
      home: Scaffold(
        body: Center(
            child: Text(
          'Hello JSPang  ,非常喜欢前端，并且愿意为此奋斗一生。我希望可以出1000集免费教程。',
          textAlign: TextAlign.left,
          overflow: TextOverflow.ellipsis,
          maxLines: 1,
          style: TextStyle(
            fontSize: 25.0,
            color: Color.fromARGB(255, 255, 150, 150),
            decoration: TextDecoration.underline,
            decorationStyle: TextDecorationStyle.solid,
          ),
        )),
      ),
    );
  }
}
```

##### Image

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    // TODO: implement createState
    return MaterialApp(
      title: "Text WIDGET",
      home: Scaffold(
        body: Center(
          child: Container(
            child: new Image.network(
              'http://blogimages.jspang.com/blogtouxiang1.jpg',
              repeat: ImageRepeat.repeatY,
            ),
            width: 300,
            height: 200,
            color: Colors.lightBlue,
          ),
        ),
      ),
    );
  }
}
```

##### ListView

1. 横向

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'JSPang Flutter Demo',
        home: Scaffold(
          appBar: AppBar(title: Text('ListView Widget')),
          body: Center(
            child: Container(
              height: 200,
              child: MyListView(),
            ),
          ),
        ));
  }
}

class MyListView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView(
      scrollDirection: Axis.horizontal,
      children: <Widget>[
        Container(
          width: 180,
          color: Colors.amber,
        ),
        Container(
          width: 180,
          color: Colors.lightBlue,
        ),
        Container(
          width: 180,
          color: Colors.deepPurpleAccent,
        ),
      ],
    );
  }
}
```

3. 动态数据
   
   ```
   class MyApp extends StatelessWidget {
     final List<String> items;
   
     MyApp({@required this.items}) : super();
   
     @override
     Widget build(BuildContext context) {
       return MaterialApp(
           title: 'JSPang Flutter Demo',
           home: Scaffold(
               appBar: AppBar(title: Text('ListView Widget')),
               body: ListView.builder(
                 itemCount: items.length,
                 itemBuilder: (context,index){
                   return ListTile(
                     title: Text('${items[index]}'),
                   );
                 },
               )));
     }
   }
   ```

4. gridview
   
   ```
   class MyApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return MaterialApp(
           title: 'JSPang Flutter Demo',
           home: Scaffold(
               appBar: AppBar(title: Text('ListView Widget')),
               body: GridView.count(
                 padding: EdgeInsets.all(20),
                 crossAxisSpacing: 10,
                 crossAxisCount: 4,
                 children: <Widget>[
                   Text('I am JOHN'),
                   Text('I am LI'),
                   Text('I am JON'),
                   Text('I am HEHE'),
                   Text('I am LILI'),
                   Text('I am SIYUE'),
                   Text('I am QIQI'),
                 ],
               )));
     }
   }
   ```

##### row

1. 固定
   
   ```
   class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
      // TODO: implement build
      return MaterialApp(
        title: 'Row Widget Demo',
        home: Scaffold(
          appBar: AppBar(
            title: Text("水平方向布局"),
          ),
          body: Row(
            children: <Widget>[
              RaisedButton(
                onPressed: () {},
                color: Colors.amber,
                child: Text('red Button'),
              ),
              RaisedButton(
                onPressed: () {},
                color: Colors.lightBlue,
                child: Text('lightBlue Button'),
              ),
              RaisedButton(
                onPressed: () {},
                color: Colors.redAccent,
                child: Text('redAccent Button'),
              ),
            ]
          ),
        ),
      );
    }
   }
   ```

2. 不固定
   
   ```
   class MyApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       // TODO: implement build
       return MaterialApp(
         title: 'Row Widget Demo',
         home: Scaffold(
           appBar: AppBar(
             title: Text("水平方向布局"),
           ),
           body: Row(children: <Widget>[
             Expanded(
               child: RaisedButton(
                 onPressed: () {},
                 color: Colors.lightBlue,
                 child: Text('lightBlue Button'),
               ),
             ),
             Expanded(
               child: RaisedButton(
                 onPressed: () {},
                 color: Colors.redAccent,
                 child: Text('redAccent Button'),
               ),
             ),
             Expanded(
               child: RaisedButton(
                 onPressed: () {},
                 color: Colors.deepPurpleAccent,
                 child: Text('deepPurpleAccent Button'),
               ),
             ),
           ]),
         ),
       );
     }
   }
   ```

##### coloumn

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return MaterialApp(
      title: 'Column Widget Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text("水平方向布局"),
        ),
        body: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('I am JOHN'),

            Text('I am LI'),
            Text('I am JON  I am JON  I am JON'),
            Text('I am HEHE'),
            Text('I am LILI'),
            Text('I am SIYUE'),
            Text('I am QIQI'),
          ],
        ),
      ),
    );
  }
}
```

* CrossAxisAlignment  相对于最长的布局

##### StackWidget

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var stack = Stack(
      alignment: FractionalOffset(0.5,0.8),
      children: <Widget>[
        CircleAvatar(
          backgroundImage:
              NetworkImage('http://blogimages.jspang.com/blogtouxiang1.jpg'),
          radius: 100,
        ),
        Container(
          decoration: BoxDecoration(color: Colors.blue),
          padding: EdgeInsets.all(5),
          child: Text("I am JON"),
        )
      ],
    );
    return MaterialApp(
      title: 'Column Widget Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text("水平方向布局"),
        ),
        body:Center(child: stack),
      ),
    );
  }
}
```

> FractionalOffset 0-1  相对于当前容器的 X Y轴

##### Position Widget

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var stack = Stack(
      alignment: FractionalOffset(0.1, 0.5),
      children: <Widget>[
        CircleAvatar(
          backgroundImage:
              NetworkImage('http://blogimages.jspang.com/blogtouxiang1.jpg'),
          radius: 100,
        ),
        Positioned(
          top: 10,
          left: 60,
          child: Text("I am JON"),
        ),
        Positioned(
          bottom: 10,
          right: 10,
          child: Text(
            "I am JON",
            style: TextStyle(fontSize: 25, color: Colors.lightBlue),
          ),
        ),
      ],
    );
    return MaterialApp(
      title: 'Column Widget Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            "水平方向布局",
            style: TextStyle(fontSize: 25, color: Colors.redAccent),
          ),
        ),
        body: Center(child: stack),
      ),
    );
  }
}
```

> Positioned 相对于当前容器 padding?

##### card

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var card = new Card(
      child: Column(
        children: <Widget>[
          ListTile(
            title: Text(
              "上海市 高兴区",
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: Text("john 182929739933"),
            leading: Icon(
              Icons.account_box,
              color: Colors.lightBlue,
            ),
          ),
          Divider(),
          ListTile(
            title: Text(
              "北京市 高兴区",
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: Text("elon 182929739933"),
            leading: Icon(
              Icons.account_box,
              color: Colors.lightBlue,
            ),
          ),
          Divider(),

          ListTile(
            title: Text(
              "江西省 高兴区",
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: Text("lari 182929739933"),
            leading: Icon(
              Icons.account_box,
              color: Colors.lightBlue,
            ),
          ),
        ],
      ),
    );
    return MaterialApp(
        title: "Row Widget Demo",
        home: Scaffold(
          appBar: AppBar(
            title: Text('Card布局'),
          ),
          body: Center(
            child: card,
          ),
        ));
  }
}
```

##### 页面跳转

```
class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: AppBar(title: Text('导航页面')),
      body: Center(
        child: RaisedButton(
          child: Text('查看商品详情页'),
          onPressed: () {
            Navigator.push(context,
                MaterialPageRoute(builder: (context) => new SecondScreen()));
          },
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: AppBar(
        title: Text('导航商品详情页'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('返回'),
          onPressed: (){
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}
```

#### 列表传参

```
void main() => runApp(MaterialApp(
    title: "列表数据传递",
    home: ProductList(
        products:
            List.generate(20, (i) => Product(' 商品 $i', '这是一个商品详情， 编号: $i')))));

class ProductList extends StatelessWidget {
  final List<Product> products;

  ProductList({this.products}) : super();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('商品列表'),
      ),
      body: ListView.builder(
          itemCount: products.length,
          itemBuilder: (context, index) {
            return ListTile(
              title: Text(products[index].title),
              onTap: () {
                Navigator.push(
                    context,
                    MaterialPageRoute(
                        builder: (context) =>
                            ProductDetail(product: products[index])));
              },
            );
          }),
    );
  }
}

class Product {
  final String title;
  final String description;

  Product(this.title, this.description);
}

class ProductDetail extends StatelessWidget {
  final Product product;

  ProductDetail({this.product}) : super();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('${product.title}'),
      ),
      body: Center(
        child: Text('${product.description}'),
      ),
    );
  }
}
```

##### 跳转返回

```
class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('小姐姐要电话'),
      ),
      body: Center(
        child: RouteButton(),
      ),
    );
  }
}

class RouteButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return RaisedButton(
      onPressed: () {
        _navigateToXiaoJIe(context);
      },
      child: Text('找小姐姐'),
    );
  }
}

_navigateToXiaoJIe(BuildContext context) async {
  final result = await Navigator.push(
      context, MaterialPageRoute(builder: (context) => XiaoJieJie()));

  Scaffold.of(context).showSnackBar(SnackBar(
    content: Text('$result'),
  ));
}

class XiaoJieJie extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: AppBar(
        title: Text('我是小姐姐'),
      ),
      body: Center(
        child: Column(
          children: <Widget>[
            RaisedButton(
              child: Text('可爱小姐姐'),
              onPressed: () {
                Navigator.pop(context, '可爱小姐姐 1550008883');
              },
            ),
            RaisedButton(
              child: Text('漂亮小姐姐'),
              onPressed: () {
                Navigator.pop(context, '漂亮小姐姐 1550009983');
              },
            ),
          ],
        ),
      ),
    );
  }
}
```
