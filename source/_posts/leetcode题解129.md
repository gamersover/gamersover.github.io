---
title: leetcode题解129：求根节点到叶节点数字之和
date: 2023-02-05 11:40:26
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第129题](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)

<!--more-->

## 分析

注意到当前节点的值加上父节点的值乘以10就是当前节点的值，使用深度优先遍历，遍历时按照上述计算方式会得到每个叶子节点的数值，对于中间节点的值就是左子结点加上右子节点。

在遍历下一层节点时，需要记住父节点的值，所以如果递归函数为`dfs(root, preSum)`，需要引入一个父节点的值`preSum`，而递归函数内部应该是：
1. 更新当前节点的值`curr = root.val + 10 * preSum`
2. 如果是叶子结点直接返回`curr`
3. 否则继续深入`dfs(root.left, curr)`，`dfs(root.right, curr)`，直到碰到叶子结点
4. 最终返回`dfs(root.left, curr) + dfs(root.right, curr)`


## 代码

```python
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(root, pre_sum):
            if root is None:
                return 0
            sum_ = pre_sum * 10 + root.val
            if root.left is None and root.right is None:
                return sum_

            return dfs(root.left, sum_) + dfs(root.right, sum_)

        return dfs(root, 0)
```