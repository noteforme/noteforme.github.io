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

## logic

```c
q = &p

&q = ox4000
*q =  *(&p) = p = &i
**q = *(*q) = &(&i) = 1
```

## array

```
// a[i] : a[i] = *(a+i) = *(p+i) = p[i]
// &a[i] : a[i] = a+i = p+i = &p[i]

    int a[3] = {1,2,3};
    int *p =a;
//    for (int i = 0; i < sizeof(a)/ sizeof(*(a+0)); ++i) {
//    for (int i = 0; i < sizeof(a)/ sizeof(*a); ++i) {
    for (int i = 0; i < sizeof(a)/ sizeof(a[0]); ++i) {
//        printf("%p -> %d\n",p+i,a[i]);
        printf("%p -> %d\n",p+i,*(p+1));
    }
```

https://www.bilibili.com/video/BV18p4y167Md/?spm_id_from=333.788.player.switch&vd_source=d4c5260002405798a57476b318eccac9&p=45

### two-dimensional array

Two-dimensional array chart is shown

| 00  | 01  | 02  |
| --- | --- | --- |
| 10  | 11  | 12  |

Two-dimensional array used in memory

| 00  | 01  | 01  | 10  | 11  | 12  |
| --- | --- | --- | --- | --- | --- |

```c
#include <stdio.h>

int main() {
    // Initialize 2D array with given values
    int a[2][3] = {
        {1, 2, 3},
        {3, 5, 6}
    };

    printf("Array values and pointers:\n\n");

    // Print array values and addresses
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            // Print the value
            printf("a[%d][%d] = %d\n", i, j, a[i][j]);

            // Print the memory address (pointer)
            printf("Address of a[%d][%d]: %p\n", i, j, &a[i][j]);

            printf("\n");
        }
    }

    // Additional information about the array and pointers
    printf("Base address of array a: %p\n", a);
    printf("Address of first row (a[0]): %p\n", a[0]);
    printf("Address of second row (a[1]): %p\n", a[1]);

    // Show how array can be accessed using pointer arithmetic
    printf("\nAccessing using pointer arithmetic:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            printf("*(*(a+%d)+%d) = %d\n", i, j, *(*(a+i)+j));
        }
    }
// 从a[0][j]开始遍历，先走J个位置，再走i, a[1][j]

    return 0;
}
```

a[0][0] = 1
Address of a[0][0]: 0x7ffca71b8cd0

a[0][1] = 2
Address of a[0][1]: 0x7ffca71b8cd4

a[0][2] = 3
Address of a[0][2]: 0x7ffca71b8cd8

a[1][0] = 3
Address of a[1][0]: 0x7ffca71b8cdc

a[1][1] = 5
Address of a[1][1]: 0x7ffca71b8ce0

a[1][2] = 6
Address of a[1][2]: 0x7ffca71b8ce4

Base address of array a: 0x7ffca71b8cd0
Address of first row (a[0]): 0x7ffca71b8cd0
Address of second row (a[1]): 0x7ffca71b8cdc

Accessing using pointer arithmetic:
*(*(a+0)+0) = 1
*(*(a+0)+1) = 2
*(*(a+0)+2) = 3
*(*(a+1)+0) = 3
*(*(a+1)+1) = 5
*(*(a+1)+2) = 6





```
*(*(a+i)+j)
```
