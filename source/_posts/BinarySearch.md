---
title: BinarySearch
comments: true
date: 2021-10-31 17:01:21
tags:
categories: DataStructure 
---



BinarySearch



已经有序



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



https://www.youtube.com/watch?v=6ysjqCUv3K4

https://www.youtube.com/watch?v=KXJSjte_OAI

https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#%E6%80%9D%E8%B7%AF



#### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)



这题没想到是插入left位置，最后一步right是可以移到   left位置或超过left,

https://leetcode-cn.com/problems/search-insert-position/solution/sou-suo-cha-ru-wei-zhi-by-leetcode-solution/
