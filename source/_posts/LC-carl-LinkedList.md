---
title: LC-carl-LinkedList
date: 2022-05-04 13:40:18
tags: LEETCODE
---



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
