---
title: C_MAKEFILE
date: 2025-02-04 21:57:48
tags:
categories:
    - CPP
---

# GCC

Course resources 12. [Compilation and Makefiles](https://cse.msu.edu/~cse251/lecture12.pdf)

https://cse.msu.edu/~cse251/

code.c

```c
#include <stdio.h> 
#include <math.h>

#define NumIter 5

int main() {
int i;
for (i=1; i<=NumIter; i++)
     printf("PI^%d is %f\n", i,i);
return 0;
}
```

### Preprocessing

* Removes preprocessor directives (commands that start with #)

* Producescode.i Don’tusedirectly

```
gcc -E  code.c >  code.i
```

output:   code.i

### Compiling

* Convertssourcecodetomachinelanguagewithunresolveddirectives 

* Producesthecode.obinary

```
gcc -o code.o -c code.i
```

output: code.o

### Linking

* Createsmachinelanguageexectutable

* Producesthecodebinarybylinkingwiththemathlibrary(‐lm)

```
 gcc -lm -o code code.o
```

output: code

### `-c` (Compile only)

  i think it equals gcc -E and gcc - o

- **Purpose**: Tells GCC to **compile** the source code into an object file (`.o` or `.obj`), but not to link it.

- **Usage**: This is used when you want to compile source files without producing an executable, typically to be linked later in a separate step.

- Example
  
  :
  
  ```
  bash
  ```

  Copy
  gcc -c myfile.c

```
This will produce an object file 
```

  myfile.o

```
 without linking.
```





# Makefile

https://www.gnu.org/software/make/manual/make.html



[Make/make.md · 无限十三年/CPP - Gitee.com](https://gitee.com/unlimited13/cpp/blob/master/Make/make.md)

https://www.bilibili.com/video/BV1Bv4y1J7QT



hello.cpp

```java
#include <stdio.h>

int main() {
    // Print "Hello, World!" to the console
    printf("Hello, World!\n");
    return 0;
}
```

MakeFile

```
hello: hello.cpp
	 g++ hello.cpp -o hello  
```

make
