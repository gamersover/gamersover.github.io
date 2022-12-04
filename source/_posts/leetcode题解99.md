---
title: leetcode题解99：恢复二叉搜索树
date: 2022-11-06 10:21:50
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第99题](https://leetcode.cn/problems/recover-binary-search-tree/)

<!--more-->

## 分析

二叉搜索树的中序遍历是递增的，所以如果有两个交换了，那么顺序就会发生变化；比如原序列为`1 2 3 4 5`，如果`2 4`交换，那么交换后的序列为`1 4 3 2 5`；那么怎么从变换后的序列中找出`2 4`呢？

遍历序列，如果第一次找到逆序对，则前面的数（即比较大的）记为第一个结点，后面的数（即比较小的）记为第二个结点，然后继续遍历，如果第二次遇到了逆序对，则将第二个结点更新为后面的数，也有可能只有一个逆序对，这时第二个结点就不需要更新了，因为此时一定是相邻的数交换了。

从而可以用中序遍历解决该问题，如果觉得递归写法有点难理解，可以将中序遍历和判断分开。

## 代码

```python
class Solution:
    def recoverTree(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        self.first = None
        self.second = None
        self.pred = TreeNode(float("-inf"))

        def in_order(root):
            if root is None:
                return
            in_order(root.left)
            if self.pred.val > root.val:
                if self.first is None:
                    self.first = self.pred
                self.second = root

            self.pred = root
            in_order(root.right)

        if root is None:
            return

        in_order(root)
        self.first.val, self.second.val = self.second.val, self.first.val
```