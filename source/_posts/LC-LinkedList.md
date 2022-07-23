---
title: LC-LinkedList
date: 2022-05-04 13:40:18
tags: LEETCODE
---





https://leetcode.cn/circle/discuss/jq9Zke/



#### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)



###### 迭代法

```kotlin
    fun removeElements(head: ListNode?, target: Int): ListNode? {
        val dummyHead = ListNode()
        dummyHead.next = head
        var p: ListNode? = dummyHead
        while (p?.next != null) { // 从真正的头节点 开始判断，因为实际没移动，所以没问题
            if (p.next?.`val` == target) {
                p.next = p.next?.next
            } else {
                p = p.next // 放最外面会跳过连续为target的Node  [7,7,7,7] 7
            }
        }
        return dummyHead.next
    }
```

###### 递归法

https://leetcode-cn.com/problems/remove-linked-list-elements/solution/yi-chu-lian-biao-yuan-su-by-leetcode-sol-654m/



#### [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

some notice

1. 注意这里不是dummyHead.next
2. // 注意： 之前这个放在上边的if里面
3. 注意这里之前是 index >= size



```kotlin
class MyLinkedList() {
    private val dummyHead = ListNode()
    var size = 0 //length

    fun get(index: Int): Int {
        if (index < -1 || index > size - 1) return -1 // 3.注意这里之前是 index >= size
        var pIndex = index
        var curr = dummyHead.next
        while (pIndex-- > 0) {
            curr = curr?.next
        }
        return curr?.`val` ?: -1
    }

    fun addAtHead(`val`: Int) {
        val newNode = ListNode(`val`) // 新的节点
        val headNode = dummyHead.next // 之前的头节点
        dummyHead.next = newNode  // 虚拟头节点 指向新的头节点
        if (headNode != null) {
            newNode.next = headNode // 新的头节点指向 之前的头节点
        }
        size++  // 注意： 之前这个放在上边的if里面
    }

    fun addAtTail(`val`: Int) {
        val newNode = ListNode(`val`) // 新的节点
        var p: ListNode? = dummyHead // 1.注意这里不是dummyHead.next
        while (p != null) {
            if (p.next == null) {
                p.next = newNode
                size++
                break // 必须停止，否则一直在尾部添加 新的节点
            }
            p = p.next
        }
    }

    fun addAtIndex(index: Int, `val`: Int) {
        if (index > size) return  //2. 注意这里之前是 index >= size
        val newNode = ListNode(`val`) // 新的节点
        var pIndex = index
        var curr = dummyHead  // 1.注意这里不是dummyHead.next
        while (pIndex-- > 0) {
            curr = curr.next!!
        }
        val temp = curr.next
        curr.next = newNode
        newNode.next = temp
        size++
    }

    fun deleteAtIndex(index: Int) {
        if (index >= size || index < 0) {
            return
        }
        var pIndex = index
        var curr: ListNode? = dummyHead //1. 注意这里之前是 index >= size
        while (pIndex-- > 0) {
            curr = curr?.next
        }
        if (curr?.next != null) {
            curr.next = curr.next?.next
        }
        size--   // 注意： 之前这个放在上边的if里面
    }
}
```



#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)



##### 双指针



```kotlin
fun reverseList(head: ListNode?): ListNode? {
    var pre = head
    var curr: ListNode? = null
    while (pre != null) {
        val termPre = pre.next
        pre.next = curr
        curr = pre
        if (termPre == null) { // 到了最后一个note下一个note,不能赋值了，否则pre就是null作为头节点了
            return pre
        }
        pre = termPre
    }
    return pre // 最后一个node,指向前面的node
}
```



after watching the https://leetcode.cn/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-shuang-zhi-zhen-di-gui-yao-mo-/

return curr point is better 



```kotlin
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
  fun reverseList(head: ListNode?): ListNode? {
    var pre = head
        var curr: ListNode? = null
        while (pre != null) {
            val termPre = pre.next
            pre.next = curr
            curr = pre
            pre = termPre
        }
        return curr // 最后一个node not null,指向前面
    }
}
```



##### 递归写法





#### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

##### 迭代法



```kotlin
fun swapPairs(head: ListNode?): ListNode? {
    val dumpNode = ListNode()
    dumpNode.next = head
    var temp = dumpNode
    while (temp.next?.next != null) {
        val n1 = temp.next
        val n2 = n1?.next
        n1?.next = n2?.next
        temp.next = n2
        n2?.next = n1
        temp = n1!!
    }
    return dumpNode.next
}
```

accoding the idea https://leetcode.cn/problems/swap-nodes-in-pairs/solution/liang-liang-jiao-huan-lian-biao-zhong-de-jie-di-91/

1. the subject most inportant is   temp = n1 , I didn't know how to do  the next cycle before

2. like author said  如果 `temp` 的后面没有节点或者只有一个节点，则没有更多的节点需要交换，因此结束交换. is also important



#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

##### 双指针

 fast Node go n steps firstly, then fast slow Node  go , at last slow point at the delete Node that last Node (慢指针 指向待删除指针的上一个指针) 

```kotlin
fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {
    val dumpNode = ListNode()
    dumpNode.next = head
    var fp = dumpNode
    var sp = dumpNode
    var lastIndex = n
    while (lastIndex-- > 0) {
        if (fp.next != null) {
            fp = fp.next!!
        }
    }
    while (dumpNode.next != null) {
        if (fp.next == null) { // 必须终止，否则会是死循环
            break
        }
        fp = fp.next!!
        sp = sp.next!!
    }
    sp.next = sp.next?.next
    return dumpNode.next
}
```





栈

https://leetcode.cn/problems/remove-nth-node-from-end-of-list/solution/shan-chu-lian-biao-de-dao-shu-di-nge-jie-dian-b-61/





#### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

1. 走对方的路，两条链表相加 路程相等

2. 相互第2个连接的链表走完后，下个节点都是null 循环就结束了

   

```kotlin
fun getIntersectionNode(headA: ListNode?, headB: ListNode?): ListNode? {
    var pA = headA
    var pB = headB
    if (pA == null || pB == null) {
        return null
    }
    while (pA != pB) { // 相互第2个连接的链表走完后，下个节点都是null 循环就结束了
        pA = if (pA == null) {
            headB
        } else {
            pA.next
        }

        pB = if (pB == null) {
            headA
        } else {
            pB.next
        }
    }
    return pA
}
```



https://leetcode.cn/problems/intersection-of-two-linked-lists/solution/xiang-jiao-lian-biao-by-leetcode-solutio-a8jn/

https://leetcode.cn/problems/intersection-of-two-linked-lists/solution/shuang-zhi-zhen-yyds-dai-ma-jian-ji-teng-0bch/



#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)



https://leetcode.cn/problems/linked-list-cycle-ii/solution/142-huan-xing-lian-biao-ii-jian-hua-gong-shi-jia-2/

很生动的图

![](https://img-blog.csdnimg.cn/20210318162938397.png)



因为fast已经在环里, Slow只能走一圈，fast不能跳过，所以 **slow的 步数 是 x+y 而不是 x + 若干环的长度 + y** 

根据公式推到结果 ,有环的话，先相遇，然后分别从head 相遇点 往相交点走.

下面这个答案就是拼出来的

##### 解法一

```kotlin
    fun detectCycle(head: ListNode?): ListNode? {
//        val dumpNode: ListNode? = ListNode() // 不设置的话，如果有指针next是null,赋值就有问题,这个不能有，必须从head出发的,因为fast point走两部
        var fp = head
        var sp = head
        while (true) {
            fp = fp?.next?.next
            sp = sp?.next
            if (fp == null) {
                return null // 走到底了，no cycle, 需要放前面，否则都是null的话，下面的fp == sp条件就匹配了
            } else if (fp == sp) {
                break
            }
        }
        var secondPoint = head
        while (sp?.next != null && secondPoint != sp) { // 看了随想录答案，这个可以放到上面的while循环里面,明天重写下
            secondPoint = secondPoint?.next!!
            sp = sp.next
        }
        return secondPoint
    }
```





##### 解法二

下面写法，看了随想录的思路，第二天重写的。

1. 可以在第一个循环的内部，head point 和相遇点开始走.
2. 公式推导过程理解了就好了，然后记住结论。

```kotlin
    fun detectCycle(head: ListNode?): ListNode? {
//        val dumpNode: ListNode? = ListNode() // 1.不设置?的话，如果有指针next是null,赋值就有问题,2.dumpNode这个不能有，必须从head出发的,因为fast point走两部
        var fp = head
        var sp = head
        while (fp != null && sp != null) { // 循环条件盘空就可以了，没有环就一定会走到null, 一直想到条件 fp != sp
            fp = fp.next?.next
            sp = sp.next
            if (fp!=null&&fp == sp) { // fp!=null不加，没环的话，就null==null 了
                var node1 = head
                while (sp?.next != null && node1 != sp) {
                    node1 = node1?.next!!
                    sp = sp.next
                }
                return node1
            }
        }
        return null
    }
```



复杂度分析

时间复杂度：O(N)O(N)，其中 NN 为链表中节点的数目。在最初判断快慢指针是否相遇时，\textit{slow}slow 指针走过的距离不会超过链表的总长度；随后寻找入环点时，走过的距离也不会超过链表的总长度。因此，总的执行时间为 O(N)+O(N)=O(N)O(N)+O(N)=O(N)。

空间复杂度：O(1)O(1)。我们只使用了 \textit{slow}, \textit{fast}, \textit{ptr}slow,fast,ptr 三个指针。

作者：LeetCode-Solution
链接：https://leetcode.cn/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

