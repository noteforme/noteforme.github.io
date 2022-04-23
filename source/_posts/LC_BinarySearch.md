---
title: ArrayBinarySearch_2
comments: true
date: 2021-10-31 17:01:21
tags:
categories: DataStructure 
---



BinarySearch

已经有序



https://www.youtube.com/watch?v=6ysjqCUv3K4

https://www.youtube.com/watch?v=KXJSjte_OAI



注意是 left 不是val mid = left + (right - left) / 2 而不是  val mid = right - (right - left) / 2 



### 边界

leetcode收藏了

https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-xiang-jie-by-labuladong/

https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-de-xun-huan-bu-bian-liang-zhi-yao-/

https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#%E6%80%9D%E8%B7%AF



##### 防止溢出

计算 mid 时需要防止溢出，代码中 left + (right - left) / 2 就和 (left + right) / 2 的结果相同，但是有效防止了 left 和 right 太大直接相加导致溢出。



#### 解法一

1	2	3	4	7	9	10 	find	 2

|  0   |   1    |   2   |   3    |  4   |  5   |   6   |
| :--: | :----: | :---: | :----: | :--: | :--: | :---: |
|  1   |   2    |   3   |   4    |  7   |  9   |  10   |
| Left |        |       | Middle |      |      | Right |
|      | Middle | Right |        |      |      |       |

[0,6]



```java
//解法1 [0,nums.length-1]
public int binarySearch1(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int middle = (left + right) / 2;
        if (target == nums[middle]) {
            return middle;
        } else if (target < nums[middle]) {
            right = middle - 1;
        } else {
            left = middle + 1;
        }
    }
    return -1;
}
```



#### 解法二

[0,7)

|  0   |   1    |  2   |   3    |  4   |  5   |  6   | 7     |
| :--: | :----: | :--: | :----: | :--: | :--: | :--: | ----- |
|  1   |   2    |  3   |   4    |  7   |  9   |  10  |       |
| Left |        |      | Middle |      |      |      | Right |
|      | Middle |      | Right  |      |      |      |       |



```java
// 解法2 [0,nums.length)
public int binarySearch2(int[] nums, int target) {
    int left = 0;
    int right = nums.length;
    while (left < right) {
        int middle = (left + right) / 2;
        if (target == nums[middle]) {
            return middle;
        } else if (target < nums[middle]) {
            right = middle;
        } else {
            left = middle + 1;
        }
    }
    return -1;
}
```



感觉还是解法1 不会容易出错。



#### [34.在排序数组中查找元素的第一个和最后一个位置(opens new window)](https://programmercarl.com/0034.在排序数组中查找元素的第一个和最后一个位置.html)



https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/zai-pai-xu-shu-zu-zhong-cha-zhao-yuan-su-de-di-3-4/



有符号 无符号的区别

- 对于**有**符号整数，每一次右移操作，高位补充的是**1**；
- 对于**无**符号整数，每一次右移操作，高位补充的则是**0**；



0x80000000

0xc0000000

0x40000000



https://juejin.cn/post/6844903969915944973



#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

(right - left) / 2   如果用  ((right - left) shr  2) 那么(right - left)要括起来，否则会符号优先级有问题.

这题没想到是插入left位置，最后一步right是可以移到   left位置或超过left,

https://leetcode-cn.com/problems/search-insert-position/solution/sou-suo-cha-ru-wei-zhi-by-leetcode-solution/



#### ~~69.x 的平方根~~

​	这题我的解法是这样的，但是还有问题 没通过

1. mid * mid.toLong() ,为什么
2. right - left + 1 即使退出循环

```
    fun mySqrt(x: Int): Int {
        var left = 0
        var right = x
        while (left < right) {
            val mid = left + (right - left + 1) / 2
            // 调试语句开始
//            try {
//                Thread.sleep(1000);
//            } catch (e: InterruptedException) {
//                e.printStackTrace()
//            }
//            println("left = $left, right = $right, mid = $mid")

//            链接：https://leetcode-cn.com/problems/sqrtx/solution/er-fen-cha-zhao-niu-dun-fa-python-dai-ma-by-liweiw/
            if (mid * mid == x) {
                return mid
            } else if (mid * mid.toLong() < x) {
                left = mid
            } else if (mid * mid.toLong() > x) {
                right = mid - 1
            }
        }
        return left;
    }
```



https://leetcode-cn.com/problems/sqrtx/solution/er-fen-cha-zhao-niu-dun-fa-python-dai-ma-by-liweiw/

有为什么产生死循环的讲解



367.有效的完全平方数

1. 解答

   ```kotlin
   class LC367 {
       fun isPerfectSquare(num: Int): Boolean {
           var left = 1
           var right = num
           while (left <= right) {
               var mid = left + (right - left) / 2
               if (mid * mid == num) {
                   return true
               } else if (mid * mid > num) {
                   right = mid - 1
               } else if (mid * mid < num) {
                   left = mid + 1
               }
           }
           return false
       }
   }
   ```

​		测试用例超出限制 2147483647. , 因为mid *mid 超出int

​	

