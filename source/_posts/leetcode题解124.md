---
title: leetcode题解124：二叉树中的最大路径和
date: 2023-02-04 10:28:11
tags:
    - leetcode
    - 递归
    - 二叉树
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第124题](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

<!--more-->

## 分析

对于树的问题，一般是划分为左子树和右子树求解。而对于该问题首先考虑经过根结点的路径有几种情况：
1. 从左子树开始以根节点为结束的路径
2. 从右子树开始以根节点为结束的路径
3. 经过根节点的路径（即跟结点在路径中间）

上面三种情况取路径和最大值，就是经过根结点的路径和的最大值；然后对每个节点都计算，最后取全局最大值就是结果。

那么化为子问题的时候，当前节点记为`root`，以左子结点`root.left`为根结点的话，求出以该节点为结束的路径和大最大值`left_max`，同理以右子节点`root.right`为根结点可以求出`right_max`；那么以`root`为根结点的第一种情况下的路径和就是`max(left_max, 0)+root.val`，第二种情况下的路径和就是`max(right_max, 0)+root.val`，第三种情况下路径和为`max(left_max, 0)+max(right_max, 0) + root.val`，最后这三种取最大。

所以最终的解决思路是定义一个函数f，输入为结点`root`，输出为以该结点结尾的路径的路径和最大值。从而递归如下：
1. 首先获取以左子节点为结尾的路径的路径和最大值`left_max=max(f(root.left), 0)`
2. 同理右子节点有`right_max=max(f(root.right), 0)`
3. 那么以`root`结尾的路径的路径和最大值就是`curr_max = root.val + max(left_max, right_max)`
4. 同时计算出三种情况的最大值`left_max + right_max + root.val`，将该值与全局最大值`ans`比较，取`max`即可。
5. 函数返回以`root`结尾的路径的路径和最大值`curr_max`


## 代码

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.ans = float('-inf')
        self.get_max_sum(root)
        return self.max_sum

    def get_max_sum(self, root):
        if root is None:
            return 0

        left_max = max(self.get_max_sum(root.left), 0)
        right_max = max(self.get_max_sum(root.right), 0)
        root_tail_max = root.val + max(left_max, right_max)
        self.ans = max(root.val + left_max + right_max, self.ans)
        return root_tail_max
```