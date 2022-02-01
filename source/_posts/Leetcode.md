---
title: Leetcode_Linklist
comments: true
date: 2021-08-30 21:03:37
tags:
categories: DataStructure
---



#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses)

```java
//官方 HashMap 解法更好 更清晰
public boolean isValid(String s) {
    Stack stack = new Stack();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if ((c == '(') || c == '[' || c == '{') {
            stack.push(c);
        } else {
            if (stack.isEmpty()) {
                return false;
            }
            boolean p1 = (c == ')' && (char) stack.peek() == '(');

            boolean p2 = (c == ']' && (char) stack.peek() == '[');
            boolean p3 = (c == '}' && (char) stack.peek() == '{');
            if (p1 || p2 || p3) {
                stack.pop();
            }else {
                stack.push(c);
            }
        }
    }
    if (stack.isEmpty()) {
        return true;
    }
    return false;
}
```



#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

##### 递归解法

```
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode prevHead = new ListNode(-1);
    ListNode  prev = prevHead;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            prev.next = l1;
            l1 = l1.next;
        } else {
            prev.next = l2;
            l2 = l2.next;
        }
        prev = prev.next;
    }

    if (l1 == null) {
        prev.next = l2;
    }
    if (l2 == null) {
        prev.next = l1;
    }
    return prevHead.next;
}
```

##### 迭代解法

待学习

#### [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)



```java
public class Solution_160 {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode la = headA;
        ListNode lb = headB;

        while(la!=lb){
            la = (la==null) ? headB : la.next;
            lb = (lb==null) ? headA : lb.next ;
        }
        return la;
    }
}
```





**先走自己的路，再走对方的路，最终路程一样**

可以这么理解，a，b两个链表，变更为 a+b 和 b+a，长度就相等了，然后等步遍历判断是否相等就行了

原始的数据

![](https://pic.leetcode-cn.com/1628662967-RLmciV-2.png)

修改后的数据示意图

![1.png](https://pic.leetcode-cn.com/1628662688-ZdSYRM-1.png)



!(![相交链表.png](https://pic.leetcode-cn.com/e86e947c8b87ac723b9c858cd3834f9a93bcc6c5e884e41117ab803d205ef662-%E7%9B%B8%E4%BA%A4%E9%93%BE%E8%A1%A8.png))

#### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

迭代法需要再写下 
