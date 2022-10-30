---
title: LC-backtrack-combination
date: 2022-10-30 10:27:10
tags: LEETCODE
categories: DataStructure
---



组合

组合的元素是无序的[1,2]  , [2,1]是一个组合

组合的元素是不能重复的 

<img src="https://img-blog.csdnimg.cn/20201123195242899.png" alt="77.组合1" style="zoom: 67%;" />







#### [77. 组合](https://leetcode.cn/problems/combinations/)

https://www.bilibili.com/video/BV1ti4y1L7cv

 

1. 拿到第一个元素.
2. 用上面的图很形象,在剩余的元素中取数据，和 二叉树路径很像,递归加回溯的过程.

这里一开始用的是 startIndex+1,不理解，这里回溯后,最第一次的for (i in startIndex..n)如果i ==2,或者3，或者4, 递归到底层，再回溯到第一次循环的时候，startIndex都是第一次的2,这样就导致剩余的元素不对



```kotlin
    fun combine(n: Int, k: Int): List<List<Int>> {
        backTracking(n, k, 1)
        return result
    }

    private fun backTracking(n: Int, k: Int, startIndex: Int) {
        if (path.size == k) {
            result.add(path.toMutableList())
            return
        }
        for (i in startIndex..n) {
            path.add(i)
            backTracking(n, k, i + 1) // 这里一开始用的是 startIndex+1
            path.remove(i)
        }
    }

```



##### 组合剪枝



https://www.bilibili.com/video/BV1wi4y157er

https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html#%E5%89%AA%E6%9E%9D%E4%BC%98%E5%8C%96

来举一个例子，n = 4，k = 4的话，那么第一层for循环的时候，从元素2开始的遍历都没有意义了。 在第二层for循环，从元素3开始的遍历都没有意义了。

![77.组合4](https://img-blog.csdnimg.cn/20210130194335207.png)



```kotlin
   for (i in startIndex..n) { // 这里对应模拟就是,取1,取2 ,取3,取4 ,几个子孩子的操作.
            path.add(i)
            backTracking(n, k, i + 1) // 这里一开始用的是 startIndex+1
            path.remove(i)
   }
```



剪枝就是 i< n这个范围里面做操作。

接下来看一下优化过程如下：

1. 已经选择的元素个数：path.size();
2. 还需要的元素个数为: k - path.size();
3. 在集合n中至多要从该起始位置 : n - (k - path.size()) + 1，开始遍历

为什么有个+1呢，因为包括起始位置，我们要是一个左闭的集合。

举个例子，n = 4，k = 3， 目前已经选取的元素为0（path.size为0），n - (k - 0) + 1 即 4 - ( 3 - 0) + 1 = 2。

从2开始搜索都是合理的，可以是组合[2, 3, 4]。

这里大家想不懂的话，建议也举一个例子，就知道是不是要+1了。



```kotlin
val path = ArrayList<Int>()
val result = ArrayList<List<Int>>()
fun combine(n: Int, k: Int): List<List<Int>> {
    backTracking1(n, k, 1)
    return result
}
//剪枝操作
private fun backTracking1(n: Int, k: Int, startIndex: Int) {
    if (path.size == k) {
        result.add(path.toMutableList())
        return
    }
    for (i in startIndex..(n - (k - path.size) + 1)) {
        path.add(i)
        backTracking1(n, k, i + 1) // 这里一开始用的是 startIndex+1
        path.remove(i)
    }
}
```
