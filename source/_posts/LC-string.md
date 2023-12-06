---
title: LC-string
date: 2022-06-05 11:06:28
tags: LEETCODE 
categories: DataStructure
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



这题的难点，主要就是去除 字符串中间的空格



##### 栈或双端队列解法



```kotlin
    /*   栈或双端队列解法
    *    1. 设置left , right指针，移动指针，类似移除两端的空格
    *    2. 从第一个元素开始遍历,添加到StringBuilder,直到遇到第一个空格，StringBuilder的数据加入队列,清空StringBuilder
    *    3. 直到最后一个加入队列
    *
    * if (s[left] != ' ') 中的' '是Ascii十进制32,单测中有验证
    * */

    fun reverseWords(s: String): String {
        var left = 0
        var right = s.length - 1
        while (s[left] == ' ') {
            left++
        }
        while (s[right] == ' ') {
            right--
        }

        val deque: Deque<String> = ArrayDeque()
        val sb = StringBuilder()
        for (i in left..right) {
            if (s[left] != ' ') {
                sb.append(s[left])
//            } else if (s[left] == ' ' && sb[sb.length - 1] != ' ') { // 一开始以为官方也是这样写的，其实也是下面我这种的
            } else if (s[left] == ' ' && sb.isNotEmpty()) { // 这里改成这样是因为加入 "the sky is blue" ， left==3走到了空格，
                deque.offerFirst(sb.toString())             //就初始化sb,然后left==4走到了s,如果两个空格，sb[sb.length - 1]就会越界
                sb.setLength(0)
            }
            left++
        }
        deque.offerFirst(sb.toString())
        return java.lang.String.join(" ", deque)
    }
```



官方题解，文字版的感觉更好点，因为有java的版本的，视频的c++的不用stringbuilder

https://leetcode.cn/problems/reverse-words-in-a-string/solution/fan-zhuan-zi-fu-chuan-li-de-dan-ci-by-leetcode-sol/



##### 两次反转字符串

```kotlin
/**
 * 1. 去除空格，并且用StringBuilder组合
 * 2. 翻转整个StringBuilder
 * 3. 翻转StringBuilder中的单个字符
 */

fun reverseWords2(s: String): String {
    val sb = deleteStartEndSpace(s)
    reverseStringBuilder(sb, 0, sb.length - 1)
    var wStart = 0
    var wEnd = 0
    while (wStart < sb.length) {
        if (sb.getOrElse(wEnd) { ' ' } == ' ') {
            reverseStringBuilder(sb, wStart, wEnd - 1)
            wStart = ++wEnd
        } else {
            wEnd++
        }
    }
    return sb.toString()
}

private fun deleteStartEndSpace(s: String): StringBuilder {
    var left = 0
    var right = s.length - 1
    while (s[left] == ' ') {
        left++
    }
    while (s[right] == ' ') {
        right--
    }
    val sb = StringBuilder()
    while (left <= right) { // 注意这里最右边的字符是需要添加的
        if (s[left] != ' ') {
            sb.append(s[left])
        } else if (s[left] == ' ' && s[left - 1] != ' ') { //不提前去掉空格，如果字符左边"  hello world  "有空格就会越界，
            sb.append(s[left])                            //而且StringBuilder右边空格也会添加空格，而且不好去掉
        }
        left++
    }
    return sb
}

private fun reverseStringBuilder(sb: StringBuilder, leftP: Int, rightP: Int) {
    var left = leftP
    var right = rightP
    while (left < right) {
        val temp = sb[left]
        sb.setCharAt(left, sb[right])
        sb.setCharAt(right, temp)
        left++
        right--
    }
}
```



官方解法，翻转子字符串这种更妙

    public void reverseEachWord(StringBuilder sb) {
        int n = sb.length();
        int start = 0, end = 0;
    
        while (start < n) {
            // 循环至单词的末尾
            while (end < n && sb.charAt(end) != ' ') {
                ++end;
            }
            // 翻转单词
            reverse(sb, start, end - 1);
            // 更新start，去找下一个单词
            start = end + 1;
            ++end;
        }
    }



#### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

##### 数组System.arraycopy 

深入分析用到了汇编指令,后面可以加强学习,其实底层的知识还是需要的，要不然没法深入下去了

https://blog.csdn.net/jackgo73/article/details/111866491



```kotlin
fun reverseLeftWords(s: String, n: Int): String {
    val sb = StringBuilder(s)
    for (i in 0 until n) {
        sb.append(s[i])
        sb.deleteCharAt(0)
    }
    return sb.toString()
}
```



**不能申请额外空间，只能在本串上操作**

我觉得用carl的方法更好

1. 反转区间为前n的子串
2. 反转区间为n到末尾的子串
3. 反转整个字符串

https://programmercarl.com/%E5%89%91%E6%8C%87Offer58-II.%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC



#### [28. 实现 strStr()](https://leetcode.cn/problems/implement-strstr/)

#####  暴力解法

aabaabaaf

aabaaf

整体移动底下的字串，然后匹配



```kotlin
fun strStr(haystack: String, needle: String): Int {
    var needleIndex = 0
    for (i in haystack.indices) {
        var hayIndex = i
        while (needleIndex < needle.length) {
            if (haystack.getOrNull(hayIndex) == needle[needleIndex]) {
                if (needleIndex == needle.length - 1) return i
                hayIndex++
                needleIndex++
            } else {
                needleIndex = 0
                break
            }
        }
    }
    return -1
}
```

这个暴力解法，也有更好的写法



##### kmp



https://noteforme.github.io/2021/06/17/kmp/



```kotlin
fun strStrKmp(haystack: String, needle: String): Int {
    var j = 0
    var nextArray = initNext(needle) // 初始化next数组
    for (i in haystack.indices) {       // 文本串指针i 移动
        while (j > 0 && haystack[i] != needle[j]) {
            j = nextArray[j - 1]    // 模式串 指针 根据next数组的下标回退
        }
        if (haystack[i] == needle[j]) { // 文本串 模式串对应的字符相等
            j++
        }
        if (j == needle.length) {
            return i - j + 1
        }
    }
    return -1
}

/**
 * 模式串
 * i指向后缀末尾位置
 * j 指向前缀末尾位置， 也代表 i和i之前的 最长公共前后缀的长度
 */
fun initNext(needle: String): IntArray {
    var nextArray = IntArray(needle.length)
    var j = 0 // j 前缀0开始初始化
    for (i in 1 until needle.length) { // i后缀末尾 ，从1开始初始化
        while (j > 0 && needle[i] != needle[j]) {
            j = nextArray[j - 1]            // 前一个位置下标 回退
        }
        if (needle[i] == needle[j]) j++     // 找到新的相等字符， 在之前最长公共前后缀长度 +1
        nextArray[i] = j // 更新next下标
    }
    return nextArray
}
```



#### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)



这题根据随想录的 公式 套的

https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

next[len - 1] = 7，next[len - 1] + 1 = 8 这句话的意思是. 这里数组统一减1，所以需要加1 == 8



```kotlin
fun repeatedSubstringPattern(s: String): Boolean {
    val nxtArrr = initNext(s)
    val len = s.length
    if (nxtArrr[len - 1] != 0 && len % (len - nxtArrr[len - 1]) == 0) { //这两句 是根据随想录的公式抄的
        return true
    }
    return false
}

private fun initNext(s: String): IntArray {
    var j = 0
    val nxt = IntArray(s.length)
    nxt[0] = 0
    for (i in 1 until s.length) {
        while (j > 0 && s[i] != s[j]) {
            j = nxt[j - 1]
        }
        if (s[i] == s[j]) j++
        nxt[i] = j
    }
    return nxt
}
```