---
title: leetcode题解117：填充每个节点的下一个右侧节点指针 II
date: 2022-11-24 15:48:32
tags:
    - leetcode
    - 递归
    - 二叉树
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第117题](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

<!--more-->

## 分析

对比[力扣第116题](https://caoqinping.com/2022/11/24/leetcode题解116/)的区别在于，该题不再是完美二叉树了，首先看下之前的做法：
1. `root.left.next = root.right`
2. `root.right.next = root.next.left`


对于第一步来说，如果`root`依然有左右节点，那么一模一样，否则不用连接；

对于第二步，本质是`A.next = B`，如果`root.right`存在，那么`A`就是`root.right`，否则看`root.left`是否存在，存在的话，就是`root.left`，要不然就不用连接，`B`的取值就要考虑一些情况，如果`root.next`没有`left,right`那么就找`root.next.next`，直到找到含有`left,right`的结点`node`，如果找到了这样的`node`，显然以`node.left`为最高优先级，即`node.left`存在，则`B = node.left`，否则`B = node.right`。找到了`A,B`，只需令`A.next = B`就好。

## 代码

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if root is None:
            return
        if root.left and root.right:
            root.left.next = root.right
        if root.next:
            left_ = root.right if root.right else root.left
            node, right_ = root.next, None
            while node:
                if node.left:
                    right_ = node.left
                    break
                elif node.right:
                    right_ = node.right
                    break
                else:
                    node = node.next
            if left_ and right_:
                left_.next = right_
        self.connect(root.right)
        self.connect(root.left)
        return root
```