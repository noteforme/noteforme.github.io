---
title: LC_TREE08
date: 2022-09-11 15:39:49
tags: LEETCODE 
categories: DataStructure
---



#### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

##### 层序遍历

层序遍历是没法实现的

```kotlin
    fun isSymmetric3(root: TreeNode?): Boolean {
        val queue = LinkedList<TreeNode>()
        if (root != null) {
            queue.offer(root)
        }
        while (queue.isNotEmpty()) {
            val layerSize = queue.size
            for (i in 0 until layerSize) {
//                val lastIndex = layerSize - 1 - i // 这样不可以,poll取出元素后，就少了元素越界了
                val headNode = queue.poll()

                val lastNode = queue.lastOrNull()
                if (headNode.`val` != lastNode?.`val`) {
                    return false
                }
                headNode.left?.let { queue.offer(it) }
                headNode.right?.let { queue.offer(it) }
            }
        }
        return true
    }
```

##### 递归

// 不能判断true,判断true直接返回了，判断false返回没问题，true返回就不会往下走了

```kotlin
private fun leftRightSymmetric(nodeLeft: TreeNode?, nodeRight: TreeNode?): Boolean {
    if (nodeLeft?.`val` == nodeRight?.`val`) { // 不能判断true,判断true直接返回了，判断false返回没问题，true返回就不会往下走了
        return true
    } else if (nodeLeft == nodeRight) { // = null
        return true
    }
    leftRightSymmetric(nodeLeft?.left, nodeRight?.right)
    leftRightSymmetric(nodeLeft?.right, nodeRight?.left)
    return false
}
```



https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E9%80%92%E5%BD%92%E6%B3%95

从根节点的左右子树开始，深度遍历比较



![](https://img-blog.csdnimg.cn/20210203144624414.png)

```kotlin
fun isSymmetric(root: TreeNode?): Boolean {
    return leftRightSymmetric(root?.left, root?.right)
}

private fun leftRightSymmetric(nodeLeft: TreeNode?, nodeRight: TreeNode?): Boolean {
    if (nodeLeft == null && nodeRight != null) {
        return false
    } else if (nodeLeft != null && nodeRight == null) {
        return false
    } else if (nodeLeft?.`val` != nodeRight?.`val`) {
        return false
    } else if (nodeLeft == null && nodeRight == null) {
        return true //这里需要，否则就递归 溢出了,如果根节点的左右节点为null,退出也是没问题的
    }
    val leftFlag = leftRightSymmetric(nodeLeft?.left, nodeRight?.right)
    val rightFlag = leftRightSymmetric(nodeLeft?.right, nodeRight?.left)
    return leftFlag && rightFlag
}
```

##### 队列

![](https://tva1.sinaimg.cn/large/008eGmZEly1gnwcimlj8lg30hm0bqnpd.gif)

队列，一直向下按照对称的条件添加



```kotlin
fun isSymmetric1(root: TreeNode?): Boolean {
    val queue = LinkedList<TreeNode>()
    if (root != null) { 
        root.left?.let { queue.offer(it) } // 官方解法中，根节点入队2次也可以
        root.right?.let { queue.offer(it) }
    }
    while (queue.isNotEmpty()) {
        val node1 = queue.poll()
        val node2 = queue.poll()
        if(node1==null&&node2==null){ // 一开始没写这个条件，提示超出时间限制
            return true
        }
        if (node1?.`val` != node2?.`val`) {
            return false
        }
        queue.offer(node1?.left) // 两边的节点比较
        queue.offer(node2?.right)
        queue.offer(node1?.right) //中间的节点比较
        queue.offer(node2?.left)
    }
    return true
}
```



栈也是可以的，只要保证位置靠近



#### [100. 相同的树](https://leetcode.cn/problems/same-tree/)

##### 递归

```kotlin
fun isSameTree(p: TreeNode?, q: TreeNode?): Boolean {
    if (p == null && q != null) {
        return false
    } else if (p != null && q == null) {
        return false
    } else if (p == null && q == null) {
        return true
    }
    return p?.`val` == q?.`val` && isSameTree(p?.left, q?.left) && isSameTree(p?.right, q?.right)
}
```



##### 队列

<img src="LC-TREE08/20220912130345.jpg" alt="20220912130345" style="zoom: 67%;" />

```kotlin

fun isSameTree1(p: TreeNode?, q: TreeNode?): Boolean {
    if (p == null && q != null) {
        return false
    } else if (p != null && q == null) {
        return false
    }
    val queue = LinkedList<TreeNode>()
    queue.offer(p)
    queue.offer(q)
    while (queue.isNotEmpty()) {
        val node1 = queue.poll()
        val node2 = queue.poll()
        if (node1 == null && node2 == null) { // continue后，因为null,就不会加入新的节点
            continue
        }
        if (node1?.`val` != node2?.`val`) {
            return false
        }
        queue.offer(node1?.left)
        queue.offer(node2?.left)
        queue.offer(node1?.right)
        queue.offer(node2?.right)
    }
    return true
}
```



#### [572. 另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/)



```kotlin
fun isSubtree(root: TreeNode?, subRoot: TreeNode?): Boolean {
    if (root == null && subRoot == null) {
        return true
    } else if (root == null && subRoot != null) {
        return false
    } else if (root != null && subRoot == null) {
        return false
    }
    return isSameTree(root, subRoot) || isSubtree(root?.left, subRoot) || isSubtree(root?.right, subRoot) //subRoot和root相同,subRoot和root左子树相同，subRoot和右子树相同
}

private fun isSameTree(node1: TreeNode?, node2: TreeNode?): Boolean {
    if (node1 == null && node2 == null) {
        return true
    } else if (node1 == null && node2 != null) {
        return false
    } else if (node1 != null && node2 == null) {
        return false
    }
    return node1?.`val` == node2?.`val` && isSameTree(node1?.left, node2?.left) && isSameTree(
        node1?.right,
        node2?.right
    )
}
```



#### [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)



##### DFS 普通二叉树解法 

```kotlin
fun countNodes(root: TreeNode?): Int {
    if (root == null) return 0
    val leftCount = countNodes(root.left)
    val rightCount = countNodes(root.right)
    return leftCount + rightCount + 1
}
```



##### BFS

```kotlin
fun countNodes1(root: TreeNode?): Int {
    val queue = LinkedList<TreeNode>()
    if (root != null) {
        queue.offer(root)
    }
    var count = 0
    while (queue.isNotEmpty()) {
        val size = queue.size
        for (i in 0 until size) {
            val treeNode = queue.poll()
            count++
            treeNode.left?.let { queue.offer(it) }
            treeNode.right?.let { queue.offer(it) }
        }
    }
    return count
}
```



##### DFS 完全二叉树特性

一开始打算用先序遍历，但是这样不行,

如果一颗树 是平衡的，永远是得到 1,  想了一下，这种也不属于 先序遍历

```kotlin
fun countNodes(root: TreeNode?): Int {
    val depth = 0
    return postTraversal(root, depth)
}

private fun postTraversal(root: TreeNode?, depth: Int): Int {
    if (isBalanced(root)) {
        return 2 shl (depth - 1)
    }
    val leftNums = postTraversal(root?.left, depth + 1)
    val rightNums = postTraversal(root?.right, depth + 1)
    return leftNums + rightNums + 1
}
```



##### 先序遍历

```kotlin
fun countNodes(root: TreeNode?): Int {
    if (root == null) return 0
    var leftNode = root.left
    var depthLeft = 0 //左子树深度
    while (leftNode != null) {
        leftNode = leftNode.left
        depthLeft++
    }
    var rightNode = root.right
    var depthRight = 0
    while (rightNode != null) {
        rightNode = rightNode.right
        depthRight++
    }
    if (depthLeft == depthRight) {
        return (2 shl depthLeft) - 1 // 节点数
    }
    val leftNums = countNodes(root.left)
    val rightNums = countNodes(root.right)
    return leftNums + rightNums + 1
}
```



##### 后序遍历

```kotlin
fun countNodes3(root: TreeNode?): Int {
    if (root == null) return 0
    var leftNode = root.left
    var depthLeft = 0 //左子树深度
    while (leftNode != null) {
        leftNode = leftNode.left
        depthLeft++
    }
    var rightNode = root.right
    var depthRight = 0
    while (rightNode != null) {
        rightNode = rightNode.right
        depthRight++
    }
    val leftNums = countNodes(root.left)
    val rightNums = countNodes(root.right)
    if (depthLeft == depthRight) {
        return (2 shl depthLeft) - 1 // 节点数
    }
    return leftNums + rightNums + 1
}
```



#### [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)



一开始错误的解答是这样的

```kotlin
    fun isBalanced(root: TreeNode?): Boolean {
        return postTraversal(root) != -1
    }

    private fun postTraversal(node: TreeNode?): Int {
        if (node == null) return 0
        val leftDepth = postTraversal(node.left)
        val rightDepth = postTraversal(node.right)
        val isBalance = Math.abs(leftDepth - rightDepth) <= 1
        if (isBalance) {
            return Math.max(leftDepth, rightDepth) + 1
        }
        return -1
    }
}
```

这里问题是   return -1，作为父节点的深度返回给了 depth，然后继续计算 Math.abs(leftDepth - rightDepth) <= 1，出现了问题



改进解法

```kotlin
fun isBalanced(root: TreeNode?): Boolean {
    return postTraversal(root) != -1
}

private fun postTraversal(node: TreeNode?): Int {
    if (node == null) return 0
    val leftDepth = postTraversal(node.left)
    val rightDepth = postTraversal(node.right)
    if (leftDepth == -1 || rightDepth == -1) { // 子节点已经有不是平衡的节点 直接返回，来判断
        return -1
    }
    val isBalance = Math.abs(leftDepth - rightDepth) <= 1
    if (isBalance) {
        return Math.max(leftDepth, rightDepth) + 1
    }
    return -1
}
```



随想录迭代遍历，看起来很复杂，看起来是统一解法.

https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E9%A2%98%E5%A4%96%E8%AF%9D



#### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)



##### 隐藏回溯过程

```kotlin
    fun binaryTreePaths(root: TreeNode?): List<String> {
        val result = LinkedList<String>()
        if (root == null) return result

        dfs(root, result, root.`val`.toString())
        return result
    }

    /**
     * https://www.youtube.com/watch?v=swG70SQBJ-A
     *
     * 1. String不能用 StringBuilder,存在引用传递问题，会输出
     *    ["1->2->5","1->2->5->3"]
     *
     * 2. 在先序位置 val存到path也有问题,头节点会存在重复的情况 ["1->1->2->5","1->1->2->5->3"]
     */
    private fun dfs(node: TreeNode?, result: LinkedList<String>, path: String) {
        if (node == null) return
        if (node.left == null && node.right == null) {
            result.add(path)
        }
        if (node.left != null) {
            dfs(node.left, result, "$path->${node.left.`val`}")
        }
        if (node.right != null) {
            dfs(node.right, result, "$path->${node.right.`val`}")
        }
    }
```



上面2的情况演示

```kotlin
private fun dfs(node: TreeNode?, result: LinkedList<String>, path: String?) {
    if (node == null) return
    val mPath = "$path-> ${node.`val`}"
    if (node.left == null && node.right == null) {
        result.add(mPath)
    }
    if (node.left != null) {
        dfs(node.left, result, mPath)
    }
    if (node.right != null) {
        dfs(node.right, result, mPath)
    }
}
```



##### 显示回溯

这是随想录的代码，二刷可以自己写

```kotlin
fun binaryTreePaths(root: TreeNode?): List<String>? {
    val res: MutableList<String> = ArrayList()
    if (root == null) {
        return res
    }
    val paths: MutableList<Int> = ArrayList()
    traversal(root, paths, res)
    return res
}

private fun traversal(root: TreeNode, paths: MutableList<Int>, res: MutableList<String>) {
    paths.add(root.`val`)
    // 叶子结点
    if (root.left == null && root.right == null) { // 碰到叶子节点，开始把path遍历放进string中 
        // 输出
        val sb = StringBuilder()
        for (i in 0 until paths.size - 1) {
            sb.append(paths[i]).append("->")
        }
        sb.append(paths[paths.size - 1]) //也可以放到上面一起再把"->" 删除
        res.add(sb.toString()) 
        return
    }
    if (root.left != null) {
        traversal(root.left, paths, res)
        paths.removeAt(paths.size - 1) // 回溯  
    }
    if (root.right != null) {
        traversal(root.right, paths, res)
        paths.removeAt(paths.size - 1) // 回溯
    }
}
```



```
 paths.removeAt(paths.size - 1) 最初这里没理解，都删除了，为什么,上面还能遍历所有节点，其实递归是一致先走的，碰到叶子节点的时候，遍历path, 最后才开始 paths.removeAt(paths.size - 1) 回溯 到上面. 
```



https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html

https://www.bilibili.com/video/BV1ZG411G7Dh

迭代法 后面再说吧
