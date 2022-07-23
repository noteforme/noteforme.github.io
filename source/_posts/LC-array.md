---
title: LC-array
date: 2022-05-02 13:07:27
tags: LEETCODE
---



#### Array结构



数组为什么从0开始.

```c
int foo[5] = {1,2,3,4,5};
foo[0] 
*(foo + 0) // 因为c语言指针偏移量从0开始
```



![这里写图片描述](https://img-blog.csdn.net/20161111021559583)

```
a[3][4]
a[0][0], a[0][1] , a[0][2] , a[0][3]
```



https://developer.51cto.com/article/649423.html

#### LEETCODE 26

https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/

解法： 双指针

首先注意数组是有序的，那么重复的元素一定会相邻。

要求删除重复元素，实际上就是将不重复的元素移到数组的左侧。

考虑用 2 个指针，一个在前记作 p，一个在后记作 q，算法流程如下：

比较 p 和 q 位置的元素是否相等。

如果相等，q 后移 1 位
如果不相等，将 q 位置的元素复制到 p+1 位置上，p 后移一位，q 后移 1 位
重复上述过程，直到 q 等于数组长度。

返回 p + 1，即为新数组长度。



![1.png](https://pic.leetcode-cn.com/0039d16b169059e8e7f998c618b6c2b269c2d95b02f43415350bde1f661e503a-1.png)

1. Nums[p] == nums[q]  q 后移1位
2. Nums[p] != nums[q].  Nums[p+1] = nums[q]. p 后移1位， q后移1位
3. Nums[p] != nums[q] 。。。。

上图向下的箭头，只代表步骤，不代表指针位置



```kotlin
//这个更好理解 https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/shuang-zhi-zhen-shan-chu-zhong-fu-xiang-dai-you-hu/
fun removeDuplicates1(nums: IntArray): Int {
    if (nums.isEmpty()){
        return 0
    }
    var slowIndex = 0
    for (fastIndex in 1 until nums.size) {
        if (nums[fastIndex] != nums[slowIndex]) {
            nums[slowIndex + 1] = nums[fastIndex] // 慢指针的后一个位置，放上 快指针不同的值
            slowIndex++
        }
    }
    return slowIndex+1
}


//官方解法，不好理解
fun removeDuplicates(nums: IntArray): Int {
    if (nums.isEmpty()){
        return 0
    }
    var slowIndex = 1
    for (fastIndex in 1 until nums.size) {
        if (nums[fastIndex] != nums[slowIndex - 1]) {
            nums[slowIndex] = nums[fastIndex]
            slowIndex++;
        }
    }
    return slowIndex
}

```



#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)



```kotlin
fun moveZeroes(nums: IntArray): Unit {
    var slowIndex = 0
    for (fastIndex in 1 until nums.size) {
        if (nums[fastIndex] != 0 && nums[slowIndex] == 0) { //符合条件就交换位置，慢指针让它在后面的条件移动
            nums[slowIndex] = nums[fastIndex]
            nums[fastIndex] = 0
        }
        if (nums[slowIndex] != 0) { // 慢指针 !=0就移动
            slowIndex++
        }
    }
}
```



#### [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)



###### 栈

```kotlin
    fun backspaceCompare1(s: String, t: String): Boolean {
        val sStack = Stack<Char>()
        for (i in s.indices) {
            if (s[i] == '#') {
                if (sStack.isNotEmpty()) {
                    sStack.pop()
                }
            } else {
                sStack.push(s[i])
            }
        }

        val tStack = Stack<Char>()
        for (j in t.indices) {
            if (t[j] == '#') { // 复制还不如自己写，一开始写成s[i]
                if (tStack.isNotEmpty()) { //放上面if,tStack是空的情况，会把 #添加进来
                    tStack.pop()
                }
            } else {
                tStack.push(t[j])// 复制还不如自己写，一开始写成s[i]
            }
        }
        return sStack == tStack
    }
```



###### 双指针1

```kotlin
fun backspaceCompare(s: String, t: String): Boolean {
    var i = s.length - 1
    var j = t.length - 1

    var skipS = 0
    var skipT = 0
    while (i >= 0 && j >= 0) {
        while (i >= 0) {
            if (s[i] == '#') { // 碰到这个先累计
                skipS++
                i--
            } else if (skipS > 0) { // 把#处理
                skipS--
                i--
            } else {
                break
            }
        }

        while (j >= 0) {
            if (t[j] == '#') {
                skipT++
                j--
            } else if (skipT > 0) {
                skipT--
                j--
            } else {
                break
            }
        }

        if (i >= 0 && j >= 0) { //上面有可能 i, j有可能到-1
            if (s[i] != t[j]) {
                return false
            }
        } else if (i >= 0 || j >= 0) { // i ,j有多余的，也有问题
            return false
        }
        i--
        j--
    }
    return true
}
```

https://leetcode-cn.com/problems/backspace-string-compare/solution/shuang-zhi-zhen-bi-jiao-han-tui-ge-de-zi-8fn8/



###### 双指针2

这种双指针写法也不错,只是最后的String.valueOf不同语言有出入，和这个题目意思有点对不上

```
class Solution {
    public boolean backspaceCompare(String s, String t) {
        // equals()判断返回的两个字符串是否相等
        return changeString(s).equals(changeString(t));
        
    }
    
    public static String changeString (String str) {
        // 先将字符串转为数组，方便使用双指针法
        char[] x = str.toCharArray();
        int slow = 0;
        for (int fast = 0; fast < x.length; fast++) {
            // 当x[fast] != '#'时，x[fast]覆盖x[slow]，然后slow++
            if (x[fast] != '#')
                x[slow++] = x[fast];
            // 当x[fast] = '#'且slow!=0时，slow--
            else if(x[fast] == '#' && slow != 0)
                slow--;
        }
        
        // 返回字符串
        return String.valueOf(x, 0, slow);
    }
}

https://leetcode-cn.com/problems/backspace-string-compare/solution/shuang-zhi-zhen-fa-si-lu-jian-dan-rong-y-bmn6/


```



### 有序数组的平方

#### [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

left ,right  左右两端比较大小，大的放新数组的最右端

```kotlin
fun sortedSquares(nums: IntArray): IntArray {
    var left = 0
    var right = nums.size - 1
    var index = nums.size - 1
    val arrayNew = IntArray(nums.size)
    while (left <= right) {
        val leftSquare = nums[left] * nums[left]
        val rightSquare = nums[right] * nums[right]
        if (leftSquare <= rightSquare) {
            arrayNew[index--] = rightSquare
            right--
        } else {
            arrayNew[index--] = leftSquare
            left++
        }
    }
    return arrayNew
}
```

https://leetcode-cn.com/problems/squares-of-a-sorted-array/solution/c-dong-hua-yan-shi-977-you-xu-shu-zu-de-gxlvm/



#### [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)



###### 双指针（滑动窗口）

滑动窗口主要找到移动左指针的条件



其实就是精简出暴力解法

```
    fun minSubArrayLen(target: Int, nums: IntArray): Int {
        var sum = 0
        var leftP = 0;
        var result = Int.MAX_VALUE
        for (rightP in nums.indices) {
            sum += nums[rightP]
            while (sum >= target) {
                val subLength = rightP - leftP + 1
                if (subLength < result) result = subLength
                sum -= nums[leftP++]
            }
        }
        if (result == Int.MAX_VALUE) result = 0
        return result
    }
```



**一些录友会疑惑为什么时间复杂度是O(n)**。

不要以为for里放一个while就以为是O(n^2)啊， 主要是看每一个元素被操作的次数，每个元素在滑动窗后进来操作一次，出去操作一次，每个元素都是被被操作两次，所以时间复杂度是 2 × n 也就是O(n)。

对于这个解释 o(n)其实我还是有一些不理解.

https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/209-chang-du-zui-xiao-de-zi-shu-zu-hua-dong-chua-7/



###### 暴力解法



先移动前面的，再移动后面的遍历找出。

```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int result = INT32_MAX; // 最终的结果
        int sum = 0; // 子序列的数值之和
        int subLength = 0; // 子序列的长度
        for (int i = 0; i < nums.size(); i++) { // 设置子序列起点为i
            sum = 0;
            for (int j = i; j < nums.size(); j++) { // 设置子序列终止位置为j
                sum += nums[j];
                if (sum >= s) { // 一旦发现子序列和超过了s，更新result
                    subLength = j - i + 1; // 取子序列的长度
                    result = result < subLength ? result : subLength;
                    break; // 因为我们是找符合条件最短的子序列，所以一旦符合条件就break
                }
            }
        }
        // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
        return result == INT32_MAX ? 0 : result;
    }
};

时间复杂度：O(n^2)
空间复杂度：O(1)

作者：carlsun-2
链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/209-chang-du-zui-xiao-de-zi-shu-zu-hua-dong-chua-7/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处.
```



#### [904. 水果成篮](https://leetcode-cn.com/problems/fruit-into-baskets/)

双指针（滑动窗口）

题目不好懂 [1,2,3,2,2]. , 1 ,2,3,数字大小分别表示水果树类别



```kotlin
fun totalFruit(fruits: IntArray): Int {
    var leftP = 0
    var basketList = mutableMapOf<Int, Int>() // key存元素，value存出现的次数
    var result = 0
    var subLength: Int
    for (rightP in fruits.indices) {
        val element = fruits[rightP] //快指针扫到的元素
        if (basketList.containsKey(element)) { //包含这个元素 个数+1
            basketList[element] = basketList[element]!!.plus(1)
        } else {
            basketList[element] = 1
        }

        while (true) {
            if (basketList.size > 2) { // 元素类型>2,就开始移动左指针
                basketList[fruits[leftP]] = basketList[fruits[leftP]]!!.minus(1) //左指针次数-1
                if (basketList[fruits[leftP]] == 0) {
                    basketList.remove(fruits[leftP]) //删除移动的左指针
                }
                leftP++
            } else {
                subLength = rightP - leftP + 1  // 子序列长度
                if (subLength > result) {          //比较找出最长的子序列
                    result = subLength
                }
                break
            }
        }

    }
    return result
}
```



```kotlin
上面解法
    if (basketList.containsKey(element)) { //包含这个元素 个数+1
            basketList[element] = basketList[element]!!.plus(1)
        } else {
            basketList[element] = 1
       }

可以用这一句替换
basketList[element] = basketList.getOrDefault(element, 0).plus(1)
```



#### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)



滑动窗口 (sliding window)



```kotlin
**
 * 核心思想: s的 left right,已经覆盖了t字串，那么就移动left,看看是否还在范围内
 * 通过判断T中A B C都 >0没被抵消， <0说明 s有多余这个字符个数
 */
class LC76 {
    fun minWindow(s: String, t: String): String {
        val map = mutableMapOf<Char, Int>()
        for (element in t) {
            map[element] = map.getOrDefault(element, 0).plus(1)
        }

        var leftP = 0 // 左指针
        var rightLast = 0 //最后匹配的最短的右指针
        var resultLength = Int.MAX_VALUE
        var resultStr = "" // 最短结果字符
        for (rightP in s.indices) {
            if (map.containsKey(s[rightP])) {  // t字符包含该元素， 元素+1
                map[s[rightP]] = map.getOrDefault(s[rightP], 0).minus(1)
            }
            while (isMatch(map, t)) { // 判断这个S的left right中是否含有t所有字符
                val subLength = rightP - leftP + 1
                if (subLength < resultLength) {
                    rightLast = rightP
                    resultLength = subLength
                    resultStr = s.substring(
                        leftP,
                        rightLast + 1
                    )
                }
                if (map.containsKey(s[leftP])) { // 向右移动left,如果元素是 map t中的，就+1，因为这个不算了
                    map[s[leftP]] = map.getOrDefault(s[leftP], 0).plus(1)
                }
                leftP++
            }
        }

        return resultStr//  the beginning index, inclusive ,endIndex – the ending index, exclusive. [begin,end)
    }


    /**
     * map中有元素>0,说明有元素还没有,map<0才全部抵消了
     */
    private fun isMatch(map: Map<Char, Int>, t: String): Boolean {
        for (element in t) {
            if (map[element]!! > 0) {
                return false
            }
        }
        return true
    }

}
```



视频讲解很清晰

https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-solution/



这道题参考去看看 labuladong解法,我没找到，后面再看看,



### 螺旋矩阵

#### [59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)



```kotlin
fun generateMatrix(n: Int): Array<IntArray> {
    var left = 0
    var top = 0
    var right = n - 1
    var bottom = n - 1
    var num = 1
    val target = n * n
    var arr = Array(n) { IntArray(n) }
    while (num <= target) {
        for (i in left..right) arr[top][i] = num++ //  从左到右 // 这里不是从0开始，是从left开始
        top++ // 上边向下收窄1
        for (i in top..bottom) arr[i][right] = num++ //  从上到下
        right-- // 右边收窄1
        for (i in right downTo left) arr[bottom][i] = num++ // 从右到左
        bottom--
        for (i in bottom downTo top){ // 从底到上
            arr[i][left] = num++
        }
        left++                            //收缩左边
    }
    return arr
}
```

题解根据下面这个解法，简单巧妙

https://leetcode-cn.com/problems/spiral-matrix-ii/solution/spiral-matrix-ii-mo-ni-fa-she-ding-bian-jie-qing-x/
