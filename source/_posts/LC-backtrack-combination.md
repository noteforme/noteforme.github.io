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

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。



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



https://www.bilibili.com/video/BV1wi4y157er/

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



#### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)



- 只使用数字1到9 .
- 每个数字 **最多使用一次** .



1. 从1开始选取，每次循环往后选取，如果往前选数字入[1,2], [2,1]的情况，就重复了.

2. 选取元素综合==n ,  并且是k个，装入数组。

   

我的解法if (sum == n && pathList.size == k)放一起不好 ，pathList.size到了k 个数，后面的就不用加了，达到竖向剪枝的目的。

```kotlin
private val result = ArrayList<List<Int>>()
private var sum: Int = 0 //集合的和
private val pathList = ArrayList<Int>() // 集合
fun combinationSum3(k: Int, n: Int): List<List<Int>> {
    blockTracking(k, n, 1)
    return result
}

private fun blockTracking(k: Int, n: Int, startIndex: Int) {
    if (sum == n && pathList.size == k) { // 相加之和为 n 的 k 个数的组合
        result.add(pathList.toMutableList())
        return
    }
    for (i in startIndex..9) {
        sum += i
        pathList.add(i)
        blockTracking(k, n, i + 1)
        sum -= i
        pathList.remove(i)
    }
}
```



##### 剪枝

还没想好



![216.组合总和III1](https://img-blog.csdnimg.cn/2020112319580476.png)



1. pathList.size到了k 个数，后面的就不用加了，达到竖向剪枝的目的。

2. (9 - (k - pathList.size) + 1) 分析 

   k - pathList.size :  从1开始选取,还差多少个元素

   9 - (k - pathList.size)  : 如果从1...9选取，为了避免重复，一直往大的数选取,第1次选到了8,如果k==2,那么过了8就没意义了，第一次的9就不需要选取了.

   (9 - (k - pathList.size) + 1) 处理下标值。



```kotlin
    private val result = ArrayList<List<Int>>()
    private var sum: Int = 0 //集合的和
    private val pathList = ArrayList<Int>() // 集合
    fun combinationSum3(k: Int, n: Int): List<List<Int>> {
        blockTracking(k, n, 1)
        println(result)
        return result
    }

    private fun blockTracking(k: Int, n: Int, startIndex: Int) {
        if (pathList.size == k) { //shuxiang
            if (sum == n) {
                result.add(pathList.toMutableList())
            }
            return
        }
        for (i in startIndex..(9 - (k - pathList.size) + 1)) { // 横向剪枝
            if (sum > n) { //剪枝
                break
            }
            sum += i
            pathList.add(i)
            blockTracking(k, n, i + 1)
            sum -= i
            pathList.remove(i)
        }
    }
```



```kotlin
private fun blockTracking(k: Int, n: Int, startIndex: Int) {
    if (sum > n) {
        return //sum > n 直接返回，随想录的解法，这样更好.
    }
    if (pathList.size == k) {
        if (sum == n) {
            result.add(pathList.toMutableList())
        }
        return
    }
    for (i in startIndex..(9 - (k - pathList.size) + 1)) {
        sum += i
        pathList.add(i)
        blockTracking(k, n, i + 1)
        sum -= i
        pathList.remove(i)
    }
}
```





#### 17.电话号码的字母组合

前面的题目都是在一个数组里面取，看到这个问题，这个问题是在数组里面，再取里面的数组。

问题是，如果输入"235",那么他们内部数组的索引startIndex怎么求出来.



##### String中的数字转int

```kotlin
val digits = "23"
val c = digits[0] - '0' // 50 - 48
println(digits[0].code) // ASCII是 50
println('0'.code)       //ASCII是 48
```



#### 39.组合总和

这题目和前面的区别是，可以选取重复的元素.

选了2后，第二列选5这时候不能选2了，否则就会重复了。

![](LC-backtrack-combination/20221120103956.jpg)



看了随想录分析视频，在分析代码前，根据上面的图了解到的思路.

1. 整体回溯架构和前面的都是一样的,选了2后，后面还是可以继续253, 选了5后只有53了，否则就有重复的。
2. 一开始我的解法是backTrack(candidates.toList(), target,i),传入for循环中的i，这样会导致有漏掉前面的情况。要知道for循环的部分就是树的宽度。
3. 后面的剪枝应该也是针对这部分。

```kotlin
var sum = 0
private val path = ArrayList<Int>()
private val result = ArrayList<List<Int>>()
fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {
    backTrack(candidates.toList(), target)
    return result
}

private fun backTrack(candidates: List<Int>, target: Int) {
    if (sum > target) {  // 节点和
        return
    }
    if (sum == target) { 
        result.add(path.toList())
        return
    }
    for ((i, item) in candidates.withIndex()) {
        path.add(item)
        sum += item
        backTrack(candidates.toMutableList().subList(i, candidates.size), target) // 只需要传入后面能选取的数组部分
        path.remove(item)
        sum -= item
    }
}
```



看了随想录的部分解法视频后，感觉这种更好，根据startIndex来取数组位置。传入startIndex后，后面的智能从startIndex后面取，和分割数组是一样的道理，感觉效率会更好。



```kotlin
    var sum = 0
    private val path = ArrayList<Int>()
    private val result = ArrayList<List<Int>>()
    fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {
        backTrack(candidates.toList(), target, 0)
        return result
    }
    private fun backTrack(candidates: List<Int>, target: Int, startIndex: Int) {
        if (sum > target) {
            return
        }
        if (sum == target) {
            result.add(path.toList())
            return
        }
        for (i in startIndex until candidates.size) {
            path.add(candidates[i])
            sum += candidates[i]
            backTrack(candidates, target, i) // i后分割元素，一开始写的是startIndex，这样for循环后面的就没过滤到了,只有startIndex过滤到了。
            path.remove(candidates[i])
            sum -= candidates[i]
        }
    }
```



##### 剪枝

![](LC-backtrack-combination/20221120103956.jpg)

如果target ==4

可以看这张图，如果经过排序左边的 235,取了2,3已经 >=4了，那么后面的5就不用去取了.

```kotlin
    var sum = 0
    private val path = ArrayList<Int>()
    private val result = ArrayList<List<Int>>()
    fun combinationSum(candidates: IntArray, target: Int): List<List<Int>> {
//        backTrack1(candidates.toList(), target)
        Arrays.sort(candidates) //剪枝前需要排序
        backTrack(candidates.toList(), target, 0)
        return result
    }
    private fun backTrack(candidates: List<Int>, target: Int, startIndex: Int) {
        if (sum > target) {
            return
        }
        if (sum == target) {
            result.add(path.toList())
            return
        }
        for (i in startIndex until candidates.size) {
            if (sum + candidates[i] > target) { //  剪枝操作
                return
            }
            path.add(candidates[i])
            sum += candidates[i]
            backTrack(candidates, target, i)
            path.remove(candidates[i])
            sum -= candidates[i]
        }
    }
```





```kotlin
private fun backTrack1(candidates: IntArray, target: Int, startIndex: Int, used: BooleanArray, layer: Int) {
    if (sum == target) {
        result.add(path.toList())
        println(path)
        return
    }

    for (i in startIndex until candidates.size) {
        if (sum + candidates[i] > target) { // 这样判断更好，因为是生序的数组，右边的枝没必要
            return
        }
        println("layer$layer i: $i candidates[i]: ${candidates[i]}   used: ${used[i]}")
        if (i > 0 && candidates[i] == candidates[i - 1] && !used[i - 1]) {
            println("continue ${candidates[i]}")
            continue
        }
        path.add(candidates[i])
        sum += candidates[i]
        used[i] = true
        println("path $path")
        backTrack1(candidates, target, i + 1, used, layer + 1) // 经过日志排查后, i + 1之前写的是 startIndex +1, 第二层报错
        path.remove(candidates[i])
        println("path backTrack $path")
        sum -= candidates[i]
        used[i] = false
    }
}
```



#### 40.组合总和 II

https://www.bilibili.com/video/BV12V4y1V73A/

 这一题的 [10,1,2,7,6,1,5]，有两个1，可能都和7组成了[1,7]就重复了，这里就是要去掉这种重复。



一开始想不到去重的思路 。

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20221021163812.png)

1. 看了视频，对应这个图. 重复的原因在于树宽 第二次取1的地方，因为第1次取1，已经包括了第二次取1的所有树枝。所以把第二次取1的树枝剪掉既可。
2. 剪枝条件1：candidates[i] == candidates[i - 1] 
3. 剪枝条件2: 通过设置used数组，只有used[i - 1] 是false才有意义, used[i - 1]= false ,说明数组的第一个1没有取，取的是第二个1，结果是[1,2]，导致和第一次的取第一个1和下一层取2,结果是[1,2]重复了。这样判断就可以把 第二次取1的这个枝干给剪掉。



```kotlin
var sum = 0
val path = ArrayList<Int>()
val result = ArrayList<List<Int>>()

fun combinationSum2(candidates: IntArray, target: Int): List<List<Int>> {
    Arrays.sort(candidates)
    println(candidates.joinToString())
    val used = BooleanArray(candidates.size)
    backTrack(candidates, target, 0, used, 0)
    return result
}

private fun backTrack(candidates: IntArray, target: Int, startIndex: Int, used: BooleanArray, layer: Int) {
    if (sum > target) {
        return
    }
    if (sum == target) {
        result.add(path.toList())
        println(path)
        return
    }

    for (i in startIndex until candidates.size) {
        println("layer$layer i: $i candidates[i]: ${candidates[i]}   used: ${used[i]}")
        if (i > 0 && candidates[i] == candidates[i - 1] && !used[i - 1]) {
            println("continue ${candidates[i]}")
            continue
        }
        path.add(candidates[i])
        sum += candidates[i]
        used[i] = true
        println("path $path")
        backTrack(candidates, target, i + 1, used, layer + 1) // 经过日志排查后, i + 1之前写的是 startIndex +1, 第二层报错
        path.remove(candidates[i])
        println("path backTrack $path")
        sum -= candidates[i]
        used[i] = false
    }
}
```



#### 131.分割回文串

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

没想明白 [["a","a","b"],["aa","b"]] 这个数组是怎么弄出来的。



https://www.bilibili.com/video/BV1c54y1e7k6



这题是边看答案变做出来的。





![131.分割回文串](https://code-thinking.cdn.bcebos.com/pics/131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.jpg)



根据上图可以这样理解 ，

1. DFS递归是 纵向切割， a|ab 取[a],  a|a|b 取 [a,a] , a|a|b| 最终到叶子节点取 [a,a,b]，到终点。
2. for循环是横向切割 。
3.   val str = s.substring(startIndex,i+1) // 这个也很关键，startIndex作为分割的起点



这题类似于树中符合条件的所有的路径，一旦纵向路径中，有一个不符合条件[a,ab] ab不符合，就直接回溯去其他路径.所以放if里判断递归会更好。



```kotlin
    val result = ArrayList<List<String>>()
    private val path = ArrayList<String>()
    fun partition(s: String): List<List<String>> {
        backTrack(s, 0)
        return result
    }

    private fun backTrack(s: String, startIndex: Int) {
        if (startIndex >= s.length) { // 纵向到叶子节点，就网上回溯
            result.add(path.toList())
            return //可以用
        }
        for (i in startIndex until s.length) {  //for循环是 横向切割
            if (isPalindromeNum(s,startIndex,i+1)){ // 判断startIndex,到 i+1是否是回文串
                val str = s.substring(startIndex,i+1) // 这个也很关键，startIndex作为分割的起点
                path.add(str)
              
           // backTrack(s, i + 1)    //递归是 纵向切割，也没有循环。这个放if里面可能更好.
           // path.removeAt(path.size-1)
            }else{
                continue // 如ab不是回文串，也不用继续了 
            }
            backTrack(s, i + 1)    //递归是 纵向切割，也没有循环。这个放if里面可能更好.
            path.removeAt(path.size-1)
        }
    }

    fun isPalindromeNum(str: String,start:Int,end:Int): Boolean {
        if (str.isEmpty()) {
            return false
        }
        val charArray = str.substring(start,end)
        for (i in 0 until charArray.length / 2) {
            if (charArray[i] != charArray[charArray.length - 1 - i]) {
                return false
            }
        }
        return true
    }

```



##### 优化解法

对判断是否回文串的优化.



#### 93.复原 IP 地址

我的分析

这题和上面131类似，可以通过切割方式解决。

1. ip地址都有四位，需要切割4次，所以树的深度是4。也是回溯终止条件之一，startIndex是切割线。(应该是i+1是切割线，否则只能分割一个字母)
2. 选取有效的ip, 前导不为0，只能是数字。
3. <255 , 可以限制树的宽度。



我写完代码后，切割成这样 2.5.5.2， 不是全部的数字，没想到什么方式能切割所有的数字。

s.substring(startIndex, s.length) 先分割前三个，然后判断最后一个字符串，这样就能分割所有的字母。



startIndex解释，下面这张图更清楚，第一层取 元素2.

![78.子集](https://img-blog.csdnimg.cn/202011232041348.png)





![93.复原IP地址](https://img-blog.csdnimg.cn/20201123203735933.png)



```kotlin
val result = ArrayList<String>()
val path = ArrayList<String>()
fun restoreIpAddresses(s: String): List<String> {
    backTrack(s, 0, 0)
    return result
}

private fun backTrack(s: String, startIndex: Int, layer: Int) {
  // 看了随想录解法，这里加上终止条件会更好。
    if (layer == 3) {   // 第三个分割线
        val substring = s.substring(startIndex, s.length) // 这里的方式就解决了，分割所有子串的问题，最后一次分割，直接到字符串的终点。
        if (isValid(substring)) {
            path.add(substring)
            result.add(path.toList().joinToString(separator = "."))
            path.removeAt(path.size-1) // 最后的字符串回溯
        }
        return
    }
    for (i in startIndex until s.length) {
        val substring = s.substring(startIndex, i + 1)
        if (isValid(substring)) {
            path.add(substring)
            backTrack(s, i + 1, layer + 1) // 注意我这里经常传错用 startIndex,如果第一次已经分割到了超过第2个位置，那么就应该传这个位置，
            path.removeAt(path.size-1)
        }
    }
}

// 判断字符串s在左闭又闭区间[start, end]所组成的数字是否合法
private fun isValid(s: String): Boolean {
    val start = 0
    val end = s.length - 1
    if (start > end) {
        return false;
    }
    if (s[start] == '0' && start != end) { // 0开头的数字不合法
        return false;
    }
    var num = 0;
    for (i in start..end) {
        if (s[i] > '9' || s[i] < '0') { // 遇到非数字字符不合法
            return false;
        }
        num = num * 10 + (s[i] - '0');
        if (num > 255) { // 如果大于255了不合法
            return false;
        }
    }
    return true;
}
```



####  [78. 子集](https://leetcode.cn/problems/subsets/)



![78.子集](https://img-blog.csdnimg.cn/202011232041348.png)



这题把上图画出来后，还是比较简单的。只要把所有步数情况添加就可以了。



```kotlin
val result = ArrayList<List<Int>>()
val path = ArrayList<Int>()
fun subsets(nums: IntArray): List<List<Int>> {
    backTrack(nums, 0)
    return result
}

private fun backTrack(nums: IntArray, startIndex: Int) {
    result.add(path.toList()) // 这棵树所有的步长，都添加
    for (i in startIndex until nums.size) {
        path.add(nums[i])
        backTrack(nums, i + 1) // 注意
        path.remove(nums[i])
    }
}
```





##### 90.[子集 II](https://leetcode.cn/problems/subsets-ii/description/)



![90.子集II](https://img-blog.csdnimg.cn/20201124195411977.png)





第一层 第二列取元素2的时候，此时子集就有2了，第三列再取就重复了。

从图中可以看出，同一树层上重复取2 就要过滤掉，同一树枝上就可以重复取2，因为同一树枝上元素的集合才是唯一子集！



```kotlin
class Solution {
    private val result = ArrayList<List<Int>>()
    private val path = ArrayList<Int>()
    fun subsetsWithDup(nums: IntArray): List<List<Int>> {
        Arrays.sort(nums)
        val used = BooleanArray(nums.size)
        backTrack(nums, used, 0)
        return result
    }

    private fun backTrack(nums: IntArray, used: BooleanArray, startIndex: Int) {
        result.add(path.toList())
        for (i in startIndex until nums.size) {
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue
            }
            used[i] = true
            path.add(nums[i])
            backTrack(nums,used,i+1)
            path.remove(nums[i])
            used[i] = false
        }
    }
}
```



!used[i - 1] 这个条件是判断树层的条件，否则会把树枝给剪掉了。



## 补充

本题也可以不使用used数组来去重，因为递归的时候下一个startIndex是i+1而不是0。

随想录这句没理解。



##### 491.递增子序列



1. 不能排序,所以子集中used解法不行

2. 树枝中小心判断大小。

   

https://www.bilibili.com/video/BV1EG4y1h78v/



一开始按照下面子集的解法做的,但是存在问题，子集是经过排序后的，然后nums[i] == nums[i - 1] && !used[i - 1]这样的条件判断，这题不能排序的，所以情况不一样。

```kotlin
    private fun backTrack(nums: IntArray, used: BooleanArray, startIndex: Int) {
        if (path.size > 1) {
            result.add(path.toList())
        }
        for (i in startIndex until nums.size) {
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue
            }
            if (i > 0 && nums[i] < nums[i - 1]){
                break
            }
            path.add(nums[i])
            used[i] = true
            backTrack(nums,used,i+1)
            path.remove(nums[i])
            used[i] = false
        }
    }
```



![491. 递增子序列1](https://img-blog.csdnimg.cn/20201124200229824.png)



上图可以看到，树层中是不能重复的，因为签名的 7包含后面7的所有情况。



```kotlin
    private val result = ArrayList<List<Int>>()
    private val path = ArrayList<Int>()

    fun findSubsequences(nums: IntArray): List<List<Int>> {
        backTrack(nums, 0, 0)
        return result
    }

    private fun backTrack(nums: IntArray, startIndex: Int, layer: Int) {
        if (path.size > 1) {                      //  至少有两个元素
            result.add(path.toList())
        }
        val set = mutableSetOf<Int>()             // 树层中是否包含 相同的元素。
        for (i in startIndex until nums.size) {
//            println("startIndex $startIndex set $set")
            if (layer == 0) {
                println("layer  set $set")
            }
            if ((path.isNotEmpty() && nums[i] < path.last()) || set.contains(nums[i])) { // 小于上一个元素，这个分支以下不用走了， set树层中包含相同的与元素。
                continue
            }
            set.add(nums[i])  //因为是判断树层，而且是每一层都局部会new 一个set，所以这个没有回溯。
            path.add(nums[i])
            backTrack(nums, i + 1, layer + 1)
            path.remove(nums[i])
        }
    }
```



```kotlin
private fun backTrack(nums: IntArray, startIndex: Int, layer: Int) {
    if (path.size > 1) {
        result.add(path.toList())
    }
    val used = IntArray(201) // -100 <= nums[i] <= 100 ， 包括 0
    for (i in startIndex until nums.size) {
        if ((path.isNotEmpty() && nums[i] < path.last()) || used[nums[i] + 100] == 1) {
            continue
        }
        used[nums[i] + 100] = 1
        path.add(nums[i])
        backTrack(nums, i + 1, layer + 1)
        path.remove(nums[i])
    }
}
```



