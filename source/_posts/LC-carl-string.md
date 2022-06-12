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
