---
id: 591
title: 'Bits and Bytes'
date: '2024-06-28T08:04:14+00:00'
layout: post
guid: 'https://jonblog.site/?p=591'
permalink: /2024/06/28/bits-and-bytes/
categories:
    - 'Computer Composition principles'
---





https://web.stanford.edu/class/cs101/bits-bytes.html

# Bit

* a "bit" is atomic: the smallest unit of storage
* A bit stores just a 0 or 1

# Byte

Group 8 bits together to make 1 byte

* e.g. 0 1 0 1 1 0 1 0
* One byte can store one character, e.g. 'A' or 'x' or '$'

# Bytes and Characters - ASCII Code

* ASCII is an encoding representing each typed character by a number
* Each number is stored in one byte (so the number is in 0..255)
* A is 65
* B is 66
* a is 96
* space is 32
* "Unicode" is an encoding for mandarin, greek, arabic, etc. languages, typically 2-bytes per "character"

![](https://www.asciitable.com/asciifull.gif)

https://www.asciitable.com/

# Typing, Bytes, and You

* Each letter is stored in a byte, as below
* 100 typed letters takes up 100 bytes
* When you send, say, a text message, the numbers are sent
* Text is quite compact, using few bytes, compared to images etc.
  ![](https://web.stanford.edu/class/cs101/hardware-letter-byte.jpg)

# Numeral systems conversion table

| Decimal | Binary | Octal | Hexadecimal |
| ------- | ------ | ----- | ----------- |
| 0       | 0000   | 0     | 0           |
| 1       | 0001   | 1     | 1           |
| 2       | 0010   | 2     | 2           |
| 3       | 0011   | 3     | 3           |
| 4       | 0100   | 4     | 4           |
| 5       | 0101   | 5     | 5           |
| 6       | 0110   | 6     | 6           |
| 7       | 0111   | 7     | 7           |
| 8       | 1000   | 10    | 8           |
| 9       | 1001   | 11    | 9           |
| 10      | 1010   | 12    | A           |
| 11      | 1011   | 13    | B           |
| 12      | 1100   | 14    | C           |
| 13      | 1101   | 15    | D           |
| 14      | 1110   | 16    | E           |
| 15      | 1111   | 17    | F           |

    HEX，英文全称 Hexadecimal system，表示十六进制。
    
    DEC，英文全称 Decimal system，表示十进制。
    
    OCT，英文全称 Octal system，表示八进制。
    
    BIN，英文全称 Binary system，表示二进制。