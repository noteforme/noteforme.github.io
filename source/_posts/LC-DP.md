---
title: LC-DP
date: 2023-04-02 17:32:31
tags: LEETCODE
categories: DataStructure
---



Labuladong

https://www.bilibili.com/video/BV1XV411Y7oE

随想录

https://www.bilibili.com/video/BV13Q4y197Wg



**对于动态规划问题，我将拆解为如下五步曲，这五步都搞清楚了，才能说把动态规划真的掌握了！**

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组



### 509 斐波那契数





```kotlin
    fun fib(n: Int): Int {
        val memory = IntArray(n + 1)
        if (memory[n] != 0) {
            return memory[n]
        } else if (n == 0) return 0
        else if (n == 1) return 1

        memory[n] = fib(n - 1) + fib(n - 2)
        return memory[n]
    }


    fun fib(n: Int): Int {
        if (n == 0) return 0
        if (n == 1) return 1
        return fib(n - 1) + fib(n - 2)
    }
```



还有一种lablado解法一开始没想出来,双指针应该怎么操作

```kotlin
fun fib(n: Int): Int {
    var pre1 = 0
    var pre2 = 1
    var sum = 0

    if (n == 0) return 0
    else if (n == 1) return 1
    for (i in 2..n) {
        sum = pre1 + pre2  // 得到当前n的num
        pre1 = pre2     //移动指针
        pre2 = sum
    }
    return sum
}
```



