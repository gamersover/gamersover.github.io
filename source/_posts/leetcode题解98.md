---
title: leetcode题解98：验证二叉搜索树
date: 2022-11-06 10:11:22
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第98题](https://leetcode.cn/problems/validate-binary-search-tree/)

<!--more-->

## 分析

非常简单直接的方法，就是用中序遍历，然后判断是否是递增序列即可。

这里另一种方法，二叉搜索树的特点是：左子树的所有结点一定小于根结点，右子树的所有结点一定大于根节点，所以从根结点往左走，左子树的上界需要更新，往右走，右子树的下界需要更新；从而如果定义一个函数`f(root, lower, upper)`表示以`root`为根结点的树，其所有结点是否都在下界`lower`与`upper`之间，从而可以递归判断左子树`f(root.left, lower, root.val)`，只需更新上界，同理递归判断右子树`f(root.right, root.val, right)`，只需更新下界。

其他细节可以看代码。

## 代码

```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.is_valid(root, -math.inf, math.inf)

    def is_valid(self, root, lower, upper):
        if root is None:
            return True
        if root.val <= lower or root.val >= upper:
            return False
        if not self.is_valid(root.left, lower, root.val):
            return False
        if not self.is_valid(root.right, root.val, upper):
            return False
        return True
```