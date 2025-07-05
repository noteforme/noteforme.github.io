---
title: C-Stream
date: 2025-05-06 21:57:48
tags:
categories:
    - CPP
---

# # help

**man -S3 getopt**

https://www.coursera.org/learn/interacting-system-managing-memory/supplement/IP2c9/complex-option-processing

# Opening a File

| Mode | Read and/or write | Does not exist? | Truncate? | Position  |
| ---- | ----------------- | --------------- | --------- | --------- |
| r    | read only         | fails           | no        | beginning |
| r+   | read/write        | fails           | no        | beginning |
| w    | write only        | created         | yes       | beginning |
| w+   | read/write        | created         | yes       | beginning |
| a    | writing           | created         | no        | end       |
| a+   | read/write        | created         | no        | end       |

https://www.coursera.org/learn/interacting-system-managing-memory/supplement/Aj8DG/opening-a-file

## read file

```c
#include <stdio.h>
#include <string.h>
int main(int argc, char ** argv){
 if (argc !=2) {
   printf("arg error");
 }
 FILE * f  = fopen(argv[1],"r");
 if (f==NULL) {
  printf("file open failed");
 }
 int c;

 while ((c=fgetc(f))!=EOF) {
  printf("%c",c);  
 }
}
```

output 

fjeijfi jfief

./file test.txt

## fgets

```c
#include <stdlib.h>
# define LINE_SIZE 5

#include <stdio.h>
#include <string.h>
int main(int argc, char ** argv){
    if (argc !=2) {
        printf("arg error");
    }
    FILE * f  = fopen(argv[1],"r");
    if (f==NULL) {
        printf("file open failed");
    }
    int c;

    long total = 0;
    char line[LINE_SIZE];
    while (fgets(line,LINE_SIZE,f)!=NULL) {
        if (strchr(line,'\n')==NULL) {
            printf("Line is too long!\n");
            return EXIT_FAILURE;
        }
        total += atoi(line);
    }
    printf("The total is %ld\n",total);
    return  EXIT_SUCCESS; 
}
```

./fgets num.txt

output : Line is too long!

å

```num.txt
123

14

-56789

77
```

# Writing to Files

You can also use **fputc** to write a single character at a time, or **fputs** to write a string without any format conversions. That is, if you do **fputs("%d")** it will just print "%d"to the file directly rather than attempting to convert an integer and print the result.

C library functions may buffer the data, and not immediately request that the OS write it. Even once the application makes the requisite system calls to write the data, the OS may buffer it internally for a while before actually writing it out to the underlying hardware device.

https://www.coursera.org/learn/interacting-system-managing-memory/supplement/OFKPW/writing-to-files

# Closing Files

We will not concern ourselves with complex corrective actions when **fclose** fails—printing an error message suffices. However, you should get in the habit of checking its return value. This way, when you are working on real programs, you will check the return value by habit, and at least think about what you should do if it fails.

https://www.coursera.org/learn/interacting-system-managing-memory/supplement/DveF8/closing-files
