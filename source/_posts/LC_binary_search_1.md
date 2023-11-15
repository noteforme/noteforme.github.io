---
title: LC_binary_search_1
comments: true
date: 2021-10-31 17:01:21
tags:
categories: DataStructure 
---

correct sentence grammar

### [704. Binary Search](https://leetcode.com/problems/binary-search/)

防止溢出

计算 mid 时需要防止溢出，代码中 left + (right - left) / 2 就和 (left + right) / 2 的结果相同，但是有效防止了 left 和 right 太大直接相加导致溢出。

边界

leetcode收藏了

[力扣（LeetCode）官网 - 全球极客挚爱的技术成长平台](https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-xiang-jie-by-labuladong/)

[力扣（LeetCode）官网 - 全球极客挚爱的技术成长平台](https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-de-xun-huan-bu-bian-liang-zhi-yao-/)

[代码随想录](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#%E6%80%9D%E8%B7%AF)

First, solve the problem, and then review the one below.

https://www.youtube.com/watch?v=6ysjqCUv3K4

https://www.youtube.com/watch?v=KXJSjte_OAI

考虑到边界情况，注意是 mid 是val mid = left + (right - left) / 2 , 而不是 val mid = right - (right - left) / 2 

##### 解法一 [left,right] while(left<=right)

| 0    | 1   | 2   | 3   | 4   | 5     |
| ---- | --- | --- | --- | --- | ----- |
| -1   | 0   | 3   | 5   | 9   | 12    |
| left |     | mid |     |     | right |
|      |     |     |     |     |       |

target = 0

In my opinion, when the target is not equal to the midpoint, it's better to understand left and right as open intervals.

```
    fun search(nums: IntArray, target: Int): Int {
        var left = 0
        var right = nums.size -1
        while (left <= right) {
            val mid = left + (right - left) / 2 // consider left right border
            if (target == nums[mid]) {
                return mid
            } else if (target > mid) {
                left = mid + 1
            } else {
                right = mid - 1
            }
            println("mid $mid  left $left  right $right")
        }
        return -1
    }
```

I was thinking about the above answer, but if left = 0 and right = 1, if (1 == target), you will never get the right answer because (left + right) / 2 equals 0,

~~so right cannot use nums.size -1.~~  "It was incorrect in this scenario because when `left` is set to `mid + 1`, which is 1, it will correctly hit the desired result on the next iteration."

1    2    3    4    7    9    10     find     2

| 0    | 1      | 2     | 3      | 4   | 5   | 6     |
|:----:|:------:|:-----:|:------:|:---:|:---:|:-----:|
| 1    | 2      | 3     | 4      | 7   | 9   | 10    |
| Left |        |       | Middle |     |     | Right |
|      | Middle | Right |        |     |     |       |

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

#### 解法二while (left < right)

left == right在区间[left, right)

[0,7)

the size is 7, so while (left < right) can easily understand, if (nums[middle]>target) right = middle can not understand at first,but we need consider outside condition is while (left < right) , next time is same , so right = middle , not middle - 1.

target ==2

| 0    | 1      | 2   | 3      | 4   | 5   | 6   | 7     |
|:----:|:------:|:---:|:------:|:---:|:---:|:---:| ----- |
| 1    | 2      | 3   | 4      | 7   | 9   | 10  |       |
| Left |        |     | Middle |     |     |     | Right |
| Left | Middle |     | Right  |     |     |     |       |
|      |        |     |        |     |     |     |       |

(0+7)/2 = 3

(0+3)/2=1

```
// 解法2 [0,nums.length)
    fun search(nums: IntArray, target: Int): Int {
        var left = 0
        var right = nums.size
        while (left < right) {
            val mid = left + (right - left) / 2
            if (nums[mid] > target) {
                right = mid
            } else if (nums[mid] < target) {
                left = mid + 1
            } else return mid
        }
        return -1
    }
```

感觉还是解法1更好理解 ,不会容易出错。

#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

(right - left) / 2 如果用 ((right - left) shr 2) 那么(right - left)要括起来，否则会符号优先级有问题.

这题没想到是插入left位置，最后一步right是可以移到 left位置或超过left,

[力扣（LeetCode）官网 - 全球极客挚爱的技术成长平台](https://leetcode-cn.com/problems/search-insert-position/solution/sou-suo-cha-ru-wei-zhi-by-leetcode-solution/)

I am thinking that, after the while loop finishes, we compare the last 'mid' value to the target. If the target is less than the 'mid' value, then return 'mid - 1'.

```
    fun searchInsert(nums: IntArray, target: Int): Int {
        var left = 0
        var right = nums.size - 1
        var mid = 0
        while (left <= right) { // 1.  missing "==" at firstly
            println("before left $left  right $right")
            mid = left + (right - left) / 2
            println("mid $mid  ")
            if (nums[mid] == target) {
                return mid
            } else if (target < nums[mid]) {
                right = mid - 1
            } else if (target > nums[mid]) {
                left = mid + 1
            }
            println("after left $left  right $right")
        }
        return left //或者right+1 
    }
```

#### ~~69.x 的平方根~~

​ 这题我的解法是这样的，但是还有问题 没通过

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

[力扣（LeetCode）官网 - 全球极客挚爱的技术成长平台](https://leetcode-cn.com/problems/sqrtx/solution/er-fen-cha-zhao-niu-dun-fa-python-dai-ma-by-liweiw/)

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

​ 测试用例超出限制 2147483647. , 因为mid *mid 超出int

​

#### [34.在排序数组中查找元素的第一个和最后一个位置(opens new window)](https://programmercarl.com/0034.在排序数组中查找元素的第一个和最后一个位置.html)

https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/zai-pai-xu-shu-zu-zhong-cha-zhao-yuan-su-de-di-3-4/

有符号 无符号的区别

- 对于**有**符号整数，每一次右移操作，高位补充的是**1**；
- 对于**无**符号整数，每一次右移操作，高位补充的则是**0**；

0x80000000

0xc0000000

0x40000000

https://juejin.cn/post/6844903969915944973
