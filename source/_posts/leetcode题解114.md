---
title: leetcode题解114：二叉树展开为链表
date: 2022-11-08 11:25:30
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第114题](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

<!--more-->

## 分析

用递归简单很多，假设已经有一个函数可以将以`root`为根结点的树转为链表，记该函数为`func(root)`，显然现处理左子树`func(root.left)`和右子树`func(root.right)`，这时左右子树已经变为链表了，然后就简单多了，模拟下过程，分为三步：
1. 将左子树的最右结点指向右子树的根结点
2. `root.right = root.left`
3. `root.left = null`


## 代码

```python
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if root:
            self.flatten(root.left)
            self.flatten(root.right)
            if root.left:
                left_last = root.left
                while left_last.right:
                    left_last = left_last.right
                left_last.right = root.right
                root.right = root.left
                root.left = None
```
