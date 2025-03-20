---
id: 394
title: CPLUS
date: '2024-06-08T12:35:00+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=394'
permalink: /2024/06/08/cplus/
categories:
    - CPP
---

https://en.cppreference.com/w/

https://www.youtube.com/watch?v=H4s55GgAg0I&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=7

https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#main

# C++ Linker Works

C++中 #include就代表把文件，copy到对应的文件中，如果不用下面的内联，就会爆出重复的Log方法。

inline,只copy方法的内容，不会copy方法体，像这样的

```cpp
void initLog(){
    Log("Init log");
   内联后被 
      std::cout << message << std::endl;
    替换 
}
```

Log.h

```cpp
inline void Log(const char * message);
```

Log.cpp

```cpp
#include <iostream>
#include "Log.h"

void initLog(){
    Log("Init log");
}
void Log(const char *message) { // 或者在这个方法加上，inline处理也可以
    std::cout << message << std::endl;
}
```

Math.cpp

```cpp
#include <iostream>
#include "Log.h"

static  int multiply(int a, int b){
    Log("multiply");
    return a * b;
}

int main(){
    std::cout << multiply(5,8) <<std::endl;
}
```

# header

copy all contents and paste it into that c++ file.

pragma once 
https://www.youtube.com/watch?v=9RJTQmK0YPI&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=11

Log.h

```cpp
void Log(const char * message);
void initLog(); // 头文件声明后，其他地方可以直接调用了
```

Math.cpp

```cpp
int main(){
    initLog();
}
```

# Pointer

```cpp
    int a  = 5;
//    void* pStr = &a; // 拿到变量的指针，指针不需要类型

    int* pStr =  &a;
    *pStr = 10 ;//如果给指针指向对象赋值，就需要类型

    LOG(a)
```

output : 10

# REFERENCES

```cpp
void increment(int value){ // 只是copy的值过来，原来的数值没有变化
    value ++;
}

int main(){
    int a  = 5;
    increment(a);
    LOG(a);
}
```

output: 5
可以看到，a数值没有增加

## 指针传值

```cpp
void increment(int* value){
    (*value) ++;
}

int main(){
    int a  = 5;
    increment(&a);
    LOG(a)
}
```

output : 6

## reference

demo1

```cpp
void increment(int& value){
    value ++;
}

int main(){
    int a  = 5;
    increment(a);
    LOG(a)
}
```

output : 6
可以看到，传递引用，操作的是同一个内存地址

demo2

```cpp
    int a  = 5;
    int b = 8;
    int & ref = a; //定义ref   a
    ref = b;        // 操作引用ref,类似于操作a
    LOG(a)
    LOG(b)
```

output: 8 8

操作指针

```cpp
int main(){
    int a  = 5;
    int b = 8;

    int * ref = &a; // 拿到a的指针，赋值给ref
    *ref = 2;            // 操作ref指向的值
    ref = &b;
    *ref = 1;
    LOG(a)
    LOG(b)
}
```

output , 2 , 1

# CLASSES

### move() outside class

```cpp
class Player {
public:
    int x, y;
    int speed;
};

void move(Player& player,int xa ,int ya){
    player.x += xa * player.speed;
    player.y += ya * player.speed;
}


int main() {

    Player player;
    player.x = 5;
    player.speed = 10;
    move(player,1,-1);
    LOG(player.x)
}
```

### move() inside class

可以看到player不需要再传入了

```cpp
class Player {
public:
    int x, y;
    int speed;

    void move(int xa, int ya) {
        x += xa * speed;
        y += ya * speed;
    }
};

int main() {
    Player player;
    player.x = 5;
    player.speed = 10;
    player.move(1, -1);
    LOG(player.x)
}
```

# CLASSES vs STRUCTS

没什么区别， c++为了兼容c
struct is  publicly defaultly 
class is  private defaultly 

# Write a C++ Class

```cpp
#include <iostream>

class Log {
public:
    const int LogLevelError = 0;
    const int LogLevelWarning = 1;
    const int LogLevelInfo = 2;

private:
    int m_LogLevel = LogLevelInfo;
public:
    void setLevel(int level) {
        m_LogLevel = level;
    }

    void error(const char *message) {
        if (m_LogLevel >= LogLevelError) {
            std::cout << "[error]:" << message << std::endl;
        }
    }

    void warn(const char *message) {
        if (m_LogLevel >= LogLevelWarning){
            std::cout << "[WARNING]:" << message << std::endl;
        }
    }

    void info(const char *message) {
        if (m_LogLevel >= LogLevelInfo){
            std::cout << "[info]:" << message << std::endl;
        }
    }
};

int main(){
    Log log;
    log.setLevel(log.LogLevelWarning);
    log.warn("Hello!");
    log.error("Hello!");
    log.info("Hello!");
}
```

# static

static variable is onnly going to visible to that c++file that you have declared in .

为了避免global variable 产生的问题。
static.cpp

```cpp
static int s_Variable = 10; //去掉static就会报错,默认private
```

main.cpp

```cpp
int s_Variable = 5;
int main(){
    std::cout<<s_Variable <<std::endl;
}
```

output 5

或者 extern

static.cpp

```cpp
static int s_Variable = 10; 
```

main.cpp

```cpp
extern int s_Variable;
int main(){
    std::cout<<s_Variable <<std::endl;
}
```

output : 10

# Static for Classes and Structs

global instance for that class
dont have class instance

```cpp
struct Entity {
    int x, y; // public

    void print() {
        std::cout << x << " , " << y << std::endl;
    }
};

int main() {
    Entity e;
    e.x = 2;
    e.y = 3;
    Entity e1 = {5, 8};

    e.print();
    e1.print();
}
```

output :
2 , 3
5 , 8

```cpp
struct Entity {
    static int x, y;  // setting static 

    void print() {
        std::cout << x << " , " << y << std::endl;
    }
};

int Entity::x;
int Entity::y;

int main() {
    Entity e;
    e.x = 2;
    e.y = 3;

    Entity e1;
    e1.x = 5;
    e1.y = 8;

    e.print();
    e1.print();
}
```

output:
5 , 8
5 , 8

当我们设置     static int x, y;  // setting static 
only one instance of those tow vairables acrossall instances of classed which means
when we changed second entity's x ,y ;
e and e1 pointing to the same memory

so we can refer to them like below

```cpp
struct Entity {
    static int x, y; // public

    void print() {
        std::cout << x << " , " << y << std::endl;
    }
};

int Entity::x;
int Entity::y;

int main() {
    Entity e;
    Entity::x = 2;
    Entity::y = 3;

    Entity e1;
    Entity::x = 5;
    Entity::y = 8;

    e.print();
    e1.print();
}
```

output:
5 , 8
5 , 8

### class static method

```cpp
struct Entity {
    static int x, y; // public

    static void print() {
        std::cout << x << " , " << y << std::endl;
    }
};

int Entity::x;
int Entity::y;

int main() {
    Entity e;
    Entity::x = 2;
    Entity::y = 3;

    Entity e1;
    Entity::x = 5;
    Entity::y = 8;

    Entity::print();
    Entity::print();
}
```

output:
5 , 8
5 , 8

static method cannot access non static variables.
casue static method  doens have a class instance.

and it works like this , it knows which entitiy is.

```cpp
struct Entity {
    static int x, y;
};

static void print(Entity e) {
    std::cout << e.x << " , " << e.y << std::endl;
}

int Entity::x;
int Entity::y;

int main() {
    Entity e;
    e.x = 2;
    e.y = 3;
    print(e);
}
```

# ENUMS

```cpp
class Log {
public:
    enum Level {
        ERROR = 0, WARNING, INFO
    };

    const int LogLevelError = 0;
    const int LogLevelWarning = 1;
    const int LogLevelInfo = 2;

private:
    int m_LogLevel = LogLevelInfo;
public:
    void setLevel(int level) {
        m_LogLevel = level;
    }
}
int main() {
    Log log;
    std::cout << log.INFO << std::endl;
}
```

output: 2
so we can see the value increasly itself.

# Constructors

```cpp
class Entity {
    int x, y;
    Entity(int _x, int _y) {
        x = _x;
        y = _y;
    }
};

int main() {
    Entity e(10, 20);
}
```

delete default construcator

```cpp
class Log {
public:
    Log() = delete;
}

int main() {
//    Log log;  //Call to deleted constructor of 'Log'
}
```

# Destructors

```cpp
class Entity {
    int x, y; 
    Entity(int _x, int _y) {
        x = _x;
        y = _y;
        std::cout << "Created Entity!"<< std::endl;
    }

    ~Entity(){
        std::cout << "Destroyed Entity!"<< std::endl;
    }
};



int main() {
    Entity e(10, 20);
    e.~Entity();
}
```

# Inheritance

```cpp
class Player : Entity{}

# Virtual Functions

```cpp
class Player : public Entity {
private:
    std::string m_Name;
public:
    Player(const std::string &mName) : m_Name(mName) {}

    std::string getName() { return m_Name; }
};


int main() {
    Entity *e = new Entity();
    std::cout << e->getName() << std::endl;
    Player *p = new Player("john");
    std::cout << p->getName() << std::endl;
}
```

output : 
Entity
john
符合预期

添加 Entity* entity = p; 

```cpp
int main() {
    Entity *e = new Entity();
    std::cout << e->getName() << std::endl;
    Player *p = new Player("john");
    std::cout << p->getName() << std::endl;

    Entity* entity = p;
    std::cout << entity->getName() << std::endl;
}
```

output:
Entity
john
Entity

和java 类似，预期输出 john,但是实际输出 Entity.

再看一个方法的输出

```cpp
class Entity {
public:
    std::string getName() { return "Entity"; }

    Entity() {}
};


class Player : public Entity {
private:
    std::string m_Name;
public:
    Player(const std::string &mName) : m_Name(mName) {}

    std::string getName() { return m_Name; }
};

void printName(Entity* entity){
    std::cout << entity->getName() << std::endl;
}


int main() {
    Entity *e = new Entity();
    printName(e);
    Player *p = new Player("john");
    printName(p);
}
```

output: 
Entity
Entity
和预期也是不一致

### virtual

新增 

```cpp
class Entity {
public:
    virtual std::string getName() { return "Entity"; } //virtual

    Entity() {}
};


class Player : public Entity {
private:
    std::string m_Name;
public:
    Player(const std::string &mName) : m_Name(mName) {}

    std::string getName() override{ return m_Name; } //override option add
};
```

新增virtual后
output:
Entity
john
和预期一致，类似于java多态。

# Interfaces

class 里只有virtual,说明这个class就是一个Interfaces

```cpp
class Printable {
public:
    virtual std::string getClassName() = 0;
};
class Entity : public Printable {
public:
    virtual std::string getName() { return "Entity"; } //virtual

    virtual std::string getClassName()  override { return "Entity"; }
};

class Player : public Entity {
private:
    std::string m_Name;
public:
    Player(const std::string &mName) : m_Name(mName) {}

    std::string getClassName() override { return "Player"; }
};

void print(Printable *obj) {
    std::cout << obj->getClassName() << std::endl;
}

int main() {
    Entity *e = new Entity();
    Player *p = new Player("john");
    print(e);
    print(p);
}
```

output : 
Entity
Player

# Array

```cpp
    int example[5];
    int* ptr = example;
    for (int i = 0; i < 5; ++i) {
        example[i] = 2;
    }

    example[2] = 5;
    *(ptr + 2) = 6;

    int* another = new int[5];
    for (int i = 0; i < 5; ++i) {
        another[i] = 2;
    }
```

# Strings

const

```cpp
    const char *name = "Cherno"; // will change the character
    name[2] = 'a'; //Read-only variable is not assignable
```

```cpp
void printString(std::string string) {
    string += "h";
    std::cout << string << std::endl;
}

int main() {
    std::string name = std::string("john") + " hello";
    printString(name);
    std::cout << name << std::endl;
}
```

output:
john helloh
john hello // name的h没有，数据没变化

### reference

```cpp
void printString(std::string &string) { 
    string += "h";
    std::cout << string << std::endl;
}

int main() {
    std::string name = std::string("john") + " hello";
    printString(name);
    std::cout << name << std::endl;
}
```

output:
john helloh
john helloh
值发生的变化

# const

不能修改指针指向内容的值 *a
但是指针a可以修改

```cpp
int main() {

    const int MAX_AGE = 90;
    const int *a = new int; // 不能修改指针指向内容的值,指针的值可以修改
    int const* a = new int; // 和上面写法一样
    const int* a = new int; //和上面写法一样


     int* const  a = new int; //相反: 可以修改指针指向内容的值 ,但是不能修改指针的值

    const int *const a = new int; //不能修改指针指向内容的值,也不能修改指针的值


    std::cout << "a point change before " << a << std::endl;
    a = (int *) &MAX_AGE;    
    std::cout << *a << std::endl;  
    std::cout << "a point change after  " << a << std::endl;


}
```

const int *a = new int; // 不能修改指针指向内容的值,指针的值可以修改

output:
a point change before 0x600002620030
90
a point change after  0x16b886f5c

### not pointer member

```cpp
class Entity {
private:
    int m_X, m_Y;
public:

    int getX() const {
//        m_X = 2; // const : cannot modify class member variables
        return m_X;
    }

    void setX(int x) {
        m_X = x;
    }
};
```

### pointer member

```cpp
class Entity {
private:
    int *m_X, *m_Y;
public:

    const int *const getX() const {
        return m_X; // the pointer cannot be modified and the  content m_X,m_Y
    }

    void setX(int x) {
        m_X = x;
    }
};
```

上面两种情况，类似于java. get. set

# Member Initializer Lists

```cpp
class Entity {
private:
    std::string m_Name;
public:
    Entity() {
        m_Name = "UnKnown";
    }

    Entity(const std::string &name) {
        m_Name = name;
    }

    const std::string &getName() const {
        return m_Name;
    };

};

int main() {
    Entity e0;
    std::cout<<e0.getName() <<std::endl;

    Entity e1("john");
    std::cout<<e1.getName() <<std::endl;
}
```

output:
UnKnown
john

另一种写法

```cpp
class Entity {
private:
    std::string m_Name;
    int m_Score;
public:
    Entity() : m_Name("UnKnown"), m_Score(0) {}


    Entity(const std::string &name) : m_Name(name) {}

    const std::string &getName() const {
        return m_Name;
    };

};

int main() {
    Entity e0;
    std::cout << e0.getName() << std::endl;

    Entity e1("john");
    std::cout << e1.getName() << std::endl;
}
```

可以看出这种写法更简单.而且还有另一个好处，下面来介绍.

```cpp
class Example {
public:
    Example() {
        std::cout << "Created Example Entity!" << std::endl;
    }

    Example(int x) {
        std::cout << "Created Example Entity!" << x << "!" << std::endl;
    }
};


class Entity {
private:
    std::string m_Name;
    Example m_Example; // 1.  Created Example Entity!   // execute firstly
public:
    Entity() {
        m_Name = std::string("UnKnown");
        m_Example = Example(8); //2.  execute secondly
    }
    const std::string &getName() const {
        return m_Name;
    };
};

int main() {
    Entity e0;
}
```

output:
Created Example Entity!
Created Example Entity!8!

可以看到，创建了 2个对象。

```cpp
class Entity {
private:
    std::string m_Name;
    Example m_Example;
public:
    Entity() : m_Example(8) {
        m_Name = std::string("UnKnown");
    }
    const std::string &getName() const {
        return m_Name;
    };
};

int main() {
    Entity e0;

}
```

output:
Created Example Entity!8!
这样只创建了一个对象。

# CREATE/INSTANTIATE OBJECTS

### stack

栈内存 会自动回收

```cpp
class Entity {
private:
    std::string m_Name;
public:
    Entity() : m_Name("UnKnown") {}

    Entity(const std::string &name) : m_Name(name) {}

    const std::string &getName() const {
        return m_Name;
    };
};

int main() {
    Entity *e;
    {
        Entity entity("john");
        e = &entity;
        std::cout << (*e).getName() << std::endl;
    }
}
```

IDE不一样,视频中entity 为空的情况，不知道怎么模拟出来,

### Heap

allocated on the heap requires you to manually called delete

```cpp
    Entity *e;
    {
        Entity *entity = new Entity("john");
        e = entity;
        std::cout << (*e).getName() << std::endl;
    }
```

# The NEW Keyword

 new Entity("john");

# The Arrow Operator

```cpp
class Entity {
public:
    void print() const {
        std::cout << "hello" << std::endl;
    }
};

class ScopedPtr {
private:
    Entity *m_Obj;
public:
    ScopedPtr(Entity *entity) : m_Obj(entity) {
    }

    ~ScopedPtr() {
        delete m_Obj;
    }

//    Entity *getObject() {
//        return m_Obj;
//    }

    Entity*  operator->() {
        return m_Obj;
    }
};


int main() {
//    ScopedPtr entity = new Entity(); // chatgpt也说这个会编译错误,很奇怪？
//    entity.getObject()->print();

    ScopedPtr entity = new Entity();
    entity->print();
}
```

chatgpt也说这个会编译错误
这一集视频，没看明白，包括上面的问题。