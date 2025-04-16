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

# Pointer

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

可以理解成行指针和列指针。

行指针： int[3] , 每次移动跳过3个位置。

列指针 ： 在列上进行移动，每次只移动一个位置。

```
*(a+i)  加* 相当于把行指针 变成 列指针
```

https://www.bilibili.com/video/BV18p4y167Md?spm_id_from=333.788.videopod.episodes&vd_source=d4c5260002405798a57476b318eccac9&p=46  6:00

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

## 数组指针

https://www.bilibili.com/video/BV18p4y167Md?spm_id_from=333.788.videopod.episodes&vd_source=d4c5260002405798a57476b318eccac9&p=46

```
int(*q)[3] = a; // 一个指针指向了数组
*p 指针 指向 int[3], 每次移动3个int
```

a 和q有同样的效果

```c
#include <stdio.h>
#include <stdlib.h>

int main( )
{
    int  a[2][3] = {[0][0]=1, [0][1]=2, [0][2]=3, [1][0]=4, [1][1]=5, [1][2]=6};
    int  i, j;
    int *p     = *a;
    int(*q)[3] = a;
    printf(format: "a %p %p\n", a, a + 1);
    printf(format: "q %p %p\n\n", q, q + 1);

    for (i = 0; i < 2; i++)
    {
        for (j = 0; j < 3; j++)
        {
            // printf("%p --> %d ", &a[i][j], a[i][j]);
             printf(format: "%p --> %d ", *(a + i) + j, *(*(a + i) + j));
            //printf("%p --> %d ", *(q + i) + j, *(*(q + i) + j));
        }

        printf(format: "\n");
    }
}
```

a 0x7ffd1d88e4c0 0x7ffd1d88e4cc
p 0x7ffd1d88e4c0 0x7ffd1d88e4cc

0x7ffd1d88e4c0 --> 1 0x7ffd1d88e4c4 --> 2 0x7ffd1d88e4c8 --> 3 
0x7ffd1d88e4cc --> 4 0x7ffd1d88e4d0 --> 5 0x7ffd1d88e4d4 --> 6 

从结果可以看到a 和q输出同样的结果，+1后移动一个行指针。

## 指针与字符数组

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main( )
{
    char str[] = "I love china!";
    char *p = str + 7;
    puts(str);
}
```

I love china!
china!

p 所指向的起始位置拿进来，依次向后输出，直到碰到尾0为止。

```
      char str[] = "hello";
      // str[6]

      //(F) str = "world"; str数组名是一个地址常量
      strcpy(str, "world"); // h被w覆盖，e被o,直到尾0.

      puts(str);
```

```c
  int main( )
  {
      char *str = "hello";

⚠     printf(format: "%d %d\n", sizeof(str), strlen(s: str));
   }
```

out: 8 5

64位系统，一个指针8个字节，heoolo不包括0,5个字节。

串常量不能被覆盖，只需要修改指针指向。

```c
      char str[] = "hello"; //串常量
      // str[6]

     // strcpy(str, "world");

      str = "world"; 

      puts(str);
```

https://www.bilibili.com/video/BV18p4y167Md?spm_id_from=333.788.player.switch&vd_source=d4c5260002405798a57476b318eccac9&p=47

## const与指针

常量

const float pi = 3.14 // pi这个值不能变，类似final in java

```c
/**
 *   const int a;
 *   int const a;
 *
 *   常量指针
 *   const int *p;
 *   int const *p;
 *
 *   指针常量
 *   int *const p;
 *
 *   const int *const p
 */
```

- 常量指针：**指向常量的指针**
  
  const修饰*p，指针指向的值不能变

- 指针常量：**这个指针是一个常量**
  
  const修饰指针，指针指向不能变

```c
    int i = 1;
    int j = 100;

    const int *const p = &i;

    //(F) p = &j; //不能改

    //(F) *p = 10;
```

### 常量指针

//指针指向可以改

```c
  #include <stdio.h>
⚠ #include <stdlib.h>

  #define PI 3.14

  int main()
  {
     int i = 1;
     int j = 100;
      // 常量指针
      const int *p = &i;

       p = &j; //指针指向可以改
      printf(format: "%d\n",*p);
     // printf("i = %d\n", i); 
  }
```

### 指针常量

指针指向不能变

```c
    int i = 1;
    int j = 100;

    const int *const p = &i;

    //(F) p = &j; 

    //(F) *p = 10;
```

指针指向的值不能变.指针指向也不能变

## 指针数组与数组指针

### 数组指针

```
int(*q)[3] = a; -> type name; -> int[3] *p; // 一个指针指向了数组
*p 指针 指向 int[3], 每次移动3个int
```

### 指针数组

数组里的每一个元素都是指针。

```
int * arr[3]; -> TYPE Name -> int *[3] arr;
```

```c
int main()
{
    char *name[5] = {"Follow me", "Basic", "Great", "Fortran", "Computer"};
    // 指针数组
    int   i, j, k;
    char *tmp;

    for (i = 0; i < 5 - 1; i++)
    {
        k = i;
        for (j = i + 1; j < 5; j++)
        {
            if (strcmp(name[k], name[j]) > 0)
                k = j;
        }
        if (k != j)
        {
            tmp     = name[i];
            name[i] = name[k];
            name[k] = tmp;
        }
    }

    for (int i = 0; i < 5; i++)
    {
        puts(name[i]);
    }

    exit(0);
}
```

## 多级指针

# Function

## 一维数组

int a[N] = {1,2,3,4,5,6};
int *p = a;

| 传参    | 形参接收类型 |
| ----- | ------ |
| a     | int *  |
| *a    | int    |
| a[0]  | int    |
| &a[3] | int *  |
| p[i]  | int    |
| p     | int *  |
| *p    | int    |
| p+1   | int*   |

```c
  #include <stdio.h>
  #include <stdlib.h>

  //void print_arr(int *p, int size)  
  void print_arr(int p[], int size) // 和int *p一样的效果
  {
      int i;
⚠     printf(format: "%s:%d\n", __FUNCTION__, sizeof(p));
      // out: 8 指针

      for (i = 0; i < size; i++)
          printf(format: "%d ", *(p + i));
      printf(format: "\n");
  }

int main( )
{
      int a[] = {[0]=1, [1]=3, [2]=5, [3]=7, [4]=9};

⚠      printf(format: "%s:%d\n", __FUNCTION__, sizeof(a));
      // out: 20 5*4

       print_arr(p: a, size: sizeof(a) / sizeof(*a));
}
```

## 二维数组

| 传参                   | 类型                              |
| -------------------- | ------------------------------- |
| `a[i][j]`            | int                             |
| `*(a+i)+j`           | int *                           |
| `a[i]+j` 等同于*(a+i)+j | ~~int~~ 正确答案： int *             |
| `p[i]`               | ~~int [N] *~~ 正确答案： int         |
| `*p`                 | ~~int [N] *~~ 正确答案： int         |
| `q[i][j]`            | int                             |
| `*q`                 | ~~int [N] *~~ 正确答案： int*        |
| `q`                  | int (*) [N]                     |
| `p+3`                | ~~int [N] *~~ 正确答案： int*        |
| `q+2`                | ~~int [N] *~~ 正确答案： int (*) [N] |

`int (*p)[N]` 数组指针 ： 指向数组的指针

```c
// void print_douarr1(int (*p)[N], int m, int n) 数组指针
void print_douarr1(int (*p)[N], int m, int n)
{
    printf("sizeof(p)=%d\n", sizeof(p));

    int i, j;

    for (i = 0; i < m; i++)
    {
        for (j = 0; j < n; j++)
        {
            // printf("%4d", *(*(p + i) + j));
            printf("%4d", p[i][j]);
        }
        printf("\n");
    }
}
```





// 为什么形参传的值不一样。

```c
float average_score(int *p, int n)
{
    float sum = 0;

    for (int i = 0; i < n; i++)
    {
        sum += *(p + i);
    }

    return sum / n;
}


void find_num(int (*p)[N], int num)
{
    for (int i = 0; i < N; i++)
    {
        printf("%d ", *(*(p + num) + i)); // 先行指针变列指针
    }
    printf("\n");
}

int main()
{
    int i, j;
    int a[M][N] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};

    float ave;

    // ave = average_score(*a, M * N);
    // printf("ave = %f\n", ave);

    find_num(a, 2);

}
```

I can explain the differences between these two parameter declarations in C.

In the function `average_score(int *p, int n)`, the parameter `*p` is a pointer to an integer. This means:

- It points to a single integer value
- It can also point to the first element of a 1D array of integers
- You can traverse through consecutive memory locations using pointer arithmetic (as done with `*(p + i)`)

In the function `find_num(int (*p)[N], int num)`, the parameter `int (*p)[N]` is a pointer to an array of N integers. This means:

- It points to a row of a 2D array
- Each "step" when using `p + 1` advances by N integers (a whole row), not just one integer
- To access individual elements, you need double dereferencing as seen in `*(*(p + num) + i)`

The main practical difference is how you navigate through memory:

- With `int *p`, adding 1 to the pointer moves to the next integer (4 bytes typically)
- With `int (*p)[N]`, adding 1 to the pointer moves N integers forward (N*4 bytes typically)

This is why in your `find_num` function, `*(p + num)` gives you a pointer to the beginning of row `num`, and then `*(*(p + num) + i)` accesses the individual elements in that row.





# 函数与指针

## 指针函数

[](https://github.com/sinlatansen/Linux-C-Notes/blob/main/C07-%E5%87%BD%E6%95%B0/C7-%E5%87%BD%E6%95%B0.md#%E6%8C%87%E9%92%88%E5%87%BD%E6%95%B0)

是一个**函数**，返回值是**指针**。

返回值 * 函数名 （形参）

如：`int * fun(int);`

### 函数指针

[](https://github.com/sinlatansen/Linux-C-Notes/blob/main/C07-%E5%87%BD%E6%95%B0/C7-%E5%87%BD%E6%95%B0.md#%E5%87%BD%E6%95%B0%E6%8C%87%E9%92%88)

是一个**指针**，指向**函数**。

类型 （* 指针名） （形参）

如： `int (*p)(int);`



```c
int * find_num(int (*p)[N], int num) // 学生的成绩，每行开头指这个学生，后续的列是他的成绩
{   
if(num>M-1)
        return NULL;
  return *(p+num);
}      

int main()
{
    int i, j;
    int a[M][N] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};

    float ave;

    // ave = average_score(*a, M * N);
    // printf("ave = %f\n", ave);

    find_num(a, 2);
}
          
```







https://www.bilibili.com/video/BV18p4y167Md?spm_id_from=333.788.player.switch&vd_source=d4c5260002405798a57476b318eccac9&p=59



### 函数指针数组

[](https://github.com/sinlatansen/Linux-C-Notes/blob/main/C07-%E5%87%BD%E6%95%B0/C7-%E5%87%BD%E6%95%B0.md#%E5%87%BD%E6%95%B0%E6%8C%87%E9%92%88%E6%95%B0%E7%BB%84)

由**函数指针**组成的数组。

类型 （*数组名【下标】） （形参）

如： `int (*arr[N])(int);`

### 指向指针函数的函数指针数组
