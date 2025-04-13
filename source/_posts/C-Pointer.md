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
            printf("a+%d = %p\n", i, a+i);
            printf("*(a+%d) = %p\n", i, *(a+i));
            printf("*(a+%d)+%d = %p\n", i, j,*(a+i)+j);
            printf("(a+%d+%d) = %p\n", i, j, (a+i+j));
            printf("*(*(a+%d)+%d) = %d\n", i, j, *(*(a+i)+j));
            printf("\n");
        }
    }

    return 0;
}
```

https://www.youtube.com/watch?v=sHcnvZA2u88

*(a+i) 获取的不是一个value值，仍然是一个指针。

```
Base address of array a: 0x7ffcb71b0280
Address of first row (a[0]): 0x7ffcb71b0280
Address of second row (a[1]): 0x7ffcb71b028c

Accessing using pointer arithmetic:
a+0 = 0x7ffcb71b0280
*(a+0) = 0x7ffcb71b0280
*(a+0)+0 = 0x7ffcb71b0280
(a+0+0) = 0x7ffcb71b0280
*(*(a+0)+0) = 1

a+0 = 0x7ffcb71b0280
*(a+0) = 0x7ffcb71b0280
*(a+0)+1 = 0x7ffcb71b0284  // 这个和上面下面的开始不一样
(a+0+1) = 0x7ffcb71b028c
*(*(a+0)+1) = 2

a+0 = 0x7ffcb71b0280
*(a+0) = 0x7ffcb71b0280
*(a+0)+2 = 0x7ffcb71b0288
(a+0+2) = 0x7ffcb71b0298
*(*(a+0)+2) = 3

a+1 = 0x7ffcb71b028c
*(a+1) = 0x7ffcb71b028c
*(a+1)+0 = 0x7ffcb71b028c
(a+1+0) = 0x7ffcb71b028c
*(*(a+1)+0) = 4

a+1 = 0x7ffcb71b028c
*(a+1) = 0x7ffcb71b028c
*(a+1)+1 = 0x7ffcb71b0290
(a+1+1) = 0x7ffcb71b0298
*(*(a+1)+1) = 5

a+1 = 0x7ffcb71b028c
*(a+1) = 0x7ffcb71b028c
*(a+1)+2 = 0x7ffcb71b0294
(a+1+2) = 0x7ffcb71b02a4
*(*(a+1)+2) = 6*(*(a+i)+j)

```

why a+i and *(a+i) have same pointer , but (a+i)+j and (a+i+j) have different pointer address result

This is a great question that gets to the heart of how 2D arrays and pointers work in C! Let me explain the key difference:

1. `a+i` vs `*(a+i)`:
   
   - `a+i` is a pointer to the entire row i (a pointer to an array of 3 integers)
   - `*(a+i)` dereferences that pointer, giving you a pointer to the first element of row i
   
   They appear to have the same address value when printed because they both point to the same memory location (the start of row i), but they have different types:
   
   - `a+i` is of type `int (*)[3]` (pointer to an array of 3 integers)
   - `*(a+i)` is of type `int*` (pointer to an integer)

2. `(a+i)+j` vs `(a+i+j)`:
   
   - `(a+i)+j` is invalid - it's trying to add j to a pointer to a row, which would skip j rows
   - `*(a+i)+j` is correct - it's adding j to a pointer to the first element of row i
   - `(a+i+j)` is adding j to a pointer to row i, which is interpreted as jumping j rows ahead

The difference in addresses occurs because:

- When you add j to `a+i`, you're moving j rows forward (each row is 3 integers × sizeof(int) bytes)
- When you add j to `*(a+i)`, you're moving j integers forward (each integer is sizeof(int) bytes)

This is why `(a+i+j)` and `*(a+i)+j` point to completely different memory locations. The first is jumping multiple rows ahead, while the second is moving within the same row.

For a 2D array access, the correct pointer arithmetic is `*(*(a+i)+j)`, which corresponds to `a[i][j]`.

> > > > > > > 3b4d32a0d17842bdf41fd80430caa55828af1fa4
