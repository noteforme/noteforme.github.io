---
title: C-Pointer
date: 2025-03-09 22:01:06
tags: 
categories: CPP
---





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
