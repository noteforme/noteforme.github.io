---
title: C-Pointer
date: 2025-03-09 22:01:06
tags: 
categories: CPP
---

the flow of building a C program

C源文件 - 预处理 - 编译 - 汇编 - 链接 - 可执行文件

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/C/202504051314822.png)

https://www.bilibili.com/video/BV18p4y167Md

man 3 exit

```c
#include <stdio.h>
#include <string.h>

int main() {
    char buf[512];      // Buffer to hold our modified path.
    char *p;            // Pointer used to manipulate buf.
    char path[] = "folder"; // Original path.

    // Copy the string "folder" into buf.
    strcpy(buf, path);
    // Set pointer p to the end of the string in buf.
    p = buf + strlen(buf);
    // Append a '/' character at the current position and then increment p.
    *p++ = '/';
    // Append the null terminator to complete the string.
    *p = '\0';

    // Output the modified string.
    printf("Modified path: %s\n", buf);

    return 0;
}
```

output:

`Modified path: folder/`



| pointer | ox4000 -> | ox3000 -> | ox2000 -> |     |     |
| ------- | --------- | --------- | --------- | --- | --- |
| value   |           | q         | p         | i   |     |
|         |           |           |           |     |     |

logic 



```
q = &p

&q = ox4000
*q =  *(&p) = p = &i
**q = *(*q) = &(&i) = 1
```
