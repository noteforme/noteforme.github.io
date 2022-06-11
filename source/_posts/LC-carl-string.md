---
title: LC-carl-string
date: 2022-06-05 11:06:28
tags: LEETCODE
---



#### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

双指针

```kotlin
fun reverseString(s: CharArray): Unit {
    var left = 0
    var right = s.size - 1
    while (left < right) {
        swap(s, left, right)
        left++
        right--
    }
}

fun swap(s: CharArray, left: Int, right: Int) {
    val temp = s[left]
    s[left] = s[right]
    s[right] = temp
}
```



#### [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

```kotlin
fun reverseStr(s: String, k: Int): String {
    val charArray = s.toCharArray()
    for (i in s.indices step 2 * k) {
        var left = i  // 每次的left指针设置好
        var right = (i + k - 1)   //接着设置每次的 右指针
        if (right > s.length - 1) { // 如果字符串长度小于 这次的右指针，使用字符串的右边界作为右指针
            right = s.length - 1
        }
        while (left < right) {  //反转
            LC344().swap(charArray, left, right)
            left++
            right--
        }
    }
    return String(charArray)
}
```



随想录感觉解法不太好

https://leetcode.cn/problems/reverse-string-ii/solution/gong-shui-san-xie-jian-dan-zi-fu-chuan-m-p88f/

这个解法和我的差不多，更优雅



#### [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

https://leetcode.cn/problems/ti-huan-kong-ge-lcof/solution/zhe-dao-ti-mu-zhen-de-you-zhe-yao-jian-dan-ma-qing/



#### [151. 颠倒字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)
