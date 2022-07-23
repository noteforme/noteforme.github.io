---
title: LC-stack-queue
date: 2022-07-03 12:16:17
tags: LEETCODE
---



#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)



最初写的



```kotlin
val stack = Stack<Int>() // 元素栈
val tempStack = Stack<Int>()  // 备用

fun push(x: Int) {
    stack.push(x)
}

fun pop(): Int {
    while (!stack.empty()) {
        tempStack.push(stack.pop())
    }
    var pop = 0
    if (!tempStack.isEmpty()) {
        pop = tempStack.pop() // 这里取的就是头元素
    }
    while (!tempStack.empty()) {
        stack.push(tempStack.pop())
    }
    return pop
}

fun peek(): Int {
    while (!stack.empty()) {
        tempStack.push(stack.pop())
    }
    var peek = 0
    if (!tempStack.isEmpty()) {
        peek = tempStack.peek() // 这里取的就是头元素,不会删除
    }
    while (!tempStack.empty()) {
        stack.push(tempStack.pop())
    }
    return peek
}

fun empty(): Boolean {
    return stack.empty()
}
```



在LC232基础上进行的优化，之前每次pop后，还得把元素放回去,这里只有push判断 tempStack有没有元素

https://leetcode.cn/problems/implement-queue-using-stacks/solution/wen-zi-shi-pin-de-fang-shi-xiang-xi-jiang-jie-li-2/



```kotlin
val stackIn = Stack<Int>() // 元素栈
val tempStack = Stack<Int>()  // 备用

fun push(x: Int) {
    while (!tempStack.empty()){ //
        stackIn.push(tempStack.pop())
    }
    stackIn.push(x)
}

fun pop(): Int {
    while (!stackIn.empty()) {
        tempStack.push(stackIn.pop())
    }
    var pop = -1 // 需要-1
    if (!tempStack.isEmpty()) {
        pop = tempStack.pop() // 这里取的就是头元素
    }
    return pop
}

fun peek(): Int {
    val pop = this.pop()
    stackIn.push(pop) // 复用pop，拿出来 再放回去
    return pop
}

fun empty(): Boolean {
    return tempStack.empty()&&stackIn.empty()
}
```





随想录写法

![](https://code-thinking.cdn.bcebos.com/gifs/232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97%E7%89%88%E6%9C%AC2.gif)

https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC



在LC232_01基础上进行的优化，push的时候不用每次都判断，只有stackOut是空的时候，才需要取后面的元素

```kotlin
private val stackIn = Stack<Int>() // 输入栈
private val stackOut = Stack<Int>()  // 输出栈

fun push(x: Int) {
    stackIn.push(x)
}

fun pop(): Int {
    if (stackOut.isEmpty()) {
        while (!stackIn.empty()) {
            stackOut.push(stackIn.pop())
        }
    }
    var data = -1
    if (!stackOut.isEmpty()) {
        data = stackOut.pop()
    }
    return data
}

fun peek(): Int {
    val pop = this.pop()
    stackOut.push(pop) // 复用pop，拿出来 再放回去
    return pop
}

fun empty(): Boolean {
    return stackOut.empty() && stackIn.empty()
}
```



#### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)



##### 我的解法

```kotlin
private val mainDeque = ArrayDeque<Int>()
private val tempDeque = ArrayDeque<Int>()


fun push(x: Int) {
    while (!tempDeque.isEmpty()) {
        mainDeque.offerLast(tempDeque.poll())
    }
    mainDeque.offerLast(x)
}

fun pop(): Int {
    var data = -1
    while (!mainDeque.isEmpty()) {
        if (mainDeque.size == 1) {
            data = mainDeque.poll()
        } else {
            data = mainDeque.poll()
            tempDeque.offerLast(data)
        }
    }
    while (!tempDeque.isEmpty()) {
        mainDeque.offerLast(tempDeque.poll())
    }
    return data
}

fun top(): Int {
    val pop = this.pop()
    mainDeque.offerLast(pop)
    return pop
}

fun empty(): Boolean {
    return tempDeque.isEmpty() && mainDeque.isEmpty()
}
```



##### 两个队列

https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html

https://leetcode.cn/problems/implement-stack-using-queues/solution/yong-dui-lie-shi-xian-zhan-by-leetcode-solution/

这里的视频教程更好理解.

![](https://assets.leetcode-cn.com/solution-static/225/225_fig1.gif)



```kotlin
private var mainDeque = LinkedList<Int>()
private var tempDeque = LinkedList<Int>()


fun push(x: Int) {
    tempDeque.offer(x)
    while (!mainDeque.isEmpty()) {
        tempDeque.offerLast(mainDeque.poll())
    }

    val temp = mainDeque //此时mainDeque一定是空的,不过没关系，不用再创建一个
    mainDeque = tempDeque //交换队列元素, tempDeque的元素给mainDeque
    tempDeque = temp
}

fun pop(): Int {
    return mainDeque.poll() //删除并返回元素
}

fun top(): Int {
    return mainDeque.peek() // 返回元素
}

fun empty(): Boolean {
    return tempDeque.isEmpty() && mainDeque.isEmpty()
}
```



##### 一个队列

也是上面官方教程

![](https://assets.leetcode-cn.com/solution-static/225/225_fig2.gif)



```kotlin
private var mainDeque = LinkedList<Int>()

fun push(x: Int) {
    mainDeque.offer(x)
    for (i in 0 until mainDeque.size - 1) { // 注意这里要-1,这样最后入队的就在头部.否则就没有意义了
        mainDeque.offer(mainDeque.poll())
    }
}

fun pop(): Int {
    return mainDeque.poll() //删除并返回元素
}

fun top(): Int {
    return mainDeque.peek() // 返回元素
}

fun empty(): Boolean {
    return mainDeque.isEmpty()
}
```



```kotlin
val queue = LinkedList<Int>()
queue.offer(0)
queue.offer(1)
queue.offer(2)

queue.offer(3)
for (i in 0 until queue.size - 1) {
    queue.offer(queue.poll())
    println(queue)
}

//看到这个入队结果
// [1, 2, 3, 0]
//[2, 3, 0, 1]
//[3, 0, 1, 2]
```





#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

```kotlin
fun isValid(s: String): Boolean {
    val mapOf = mapOf('(' to ')', '{' to '}', '[' to ']')
    val stack = Stack<Char>()
    s.chars().forEach {
        if (!stack.empty() && mapOf[stack.peek()] == it.toChar()) { //stack 为空 取出会抛出异常, mapOf应该取左边的符号也就是stack里的
            stack.pop()
        } else {
            stack.push(it.toChar())
        }
    }
    return stack.empty()
}
```





#### [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)



而且**在企业项目开发中，尽量不要使用递归！**在项目比较大的时候，由于参数多，全局变量等等，使用递归很容易判断不充分return的条件，非常容易无限递归（或者递归层级过深），**造成栈溢出错误（这种问题还不好排查！）**

```kotlin
fun removeDuplicates(s: String): String {
    val stack = Stack<Char>()
    s.chars().forEach {
        if(!stack.empty()&&stack.peek()==it.toChar()){
            stack.pop()
        }else{
            stack.push(it.toChar())
        }
    }
    return stack.joinToString(separator = "")
}
```



#### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

```kotlin
fun evalRPN(tokens: Array<String>): Int {
    val stack = Stack<Int>()
    tokens.forEach {
        if (it == "+" || it == "-" || it == "*" || it == "/") {
            val pop1 = stack.takeUnless { p1 -> p1.empty() }?.pop()
            val pop2 = stack.takeUnless { p2 -> p2.empty() }?.pop()
            if (pop1 != null && pop2 != null) {
                val result = getResult(it, pop2.toInt(), pop1.toInt()) // stack取的第二个数放前面
                stack.push(result)
            }
        } else {
            stack.push(it.toInt())
        }
    }
    return stack.peek()
}

private fun getResult(operator: String, pop1: Int, pop2: Int): Int {
    return when (operator) {
        "+" -> pop1 + pop2
        "-" -> pop1 - pop2
        "*" -> pop1 * pop2
        "/" -> pop1 / pop2
        else -> throw RuntimeException("oh something went wrong")
    }
}
```



#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)



##### 存储元素坐标

https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html



nums[index - k]是队列头部最大元素，此时最大元素出队列,因为最大元素一定改窗口的最左侧元素，

如果不是最大元素，因为滑动窗口可能没有k个元素,也不用担心是不是留在窗口里，这种情况下其实在下面push已经早出去了



这样更好理解 移动窗口,队列头结点需要在[i - k + 1, i]范围内，不符合则要弹出



这种也是官方解法

```kotlin
fun maxSlidingWindow2(nums: IntArray, k: Int): IntArray {
    val linkedList = LinkedList<Int>()
    val resultArray = IntArray(nums.size - (k - 1))  // 如果窗口是3个元素，结构数组就需要少两个元素
    var i = 0
    nums.forEachIndexed { index, value ->
        if (!linkedList.isEmpty() && linkedList.peek() < index - (k - 1)) { //这样更好理解 移动窗口,队列头结点需要在[i - k + 1, i]范围内，不符合则要弹出
            linkedList.pop()
        }
        while (!linkedList.isEmpty() && value > nums[linkedList.last()]) {   // 如果大于最后一个元素
            linkedList.removeLast()                                     //最后一个元素出队列
        }
        linkedList.offer(index)                                          //否则直接入队,这样保持队列元素单调递减
        if (index >= (k - 1)) { //如果是3个元素，从位置2开始存最大值，如果是1个元素，就从0存最大值
            resultArray[i++] = nums[linkedList.peek()]
        }
    }
    return resultArray
}
```



https://leetcode.cn/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-de-zui-da-zhi-bao-mu-9eci/



##### MyQueue

随想录定义Myquue,更好，单一指责，更有层次感

//  注意先将前k的元素放进队列,此时push方法，放完后已经是按照单调递减队列来的了

二刷来刷刷刷



#### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

大顶堆 看官方题解理解了

但是大顶堆 小顶堆区别不是很明白，学完树再来继续做这个题目







