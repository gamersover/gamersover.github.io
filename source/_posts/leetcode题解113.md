---
title: leetcode题解113：路径总和 II
date: 2022-11-08 11:11:46
tags:
    - leetcode
    - 二叉树
    - 递归
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第113题](https://leetcode.cn/problems/path-sum-ii/)

<!--more-->

## 分析

典型的dfs了，函数参数应该有 当前结点，前面的路径以及剩下的和（总和减去前面路径的和） 即`dfs(root, sum, path)`，接下来需要考虑什么是否路径正确，以及函数退出条件。

显然当`root`没有左右结点，且`root`的值等于`sum`时，表示路径总和等于给的总和，此时`path`添加`root`结点就是一条目标路径；

而当`root`有左节点或右节点时，显然继续递归，这时现将`path`路径添加上`root`结点，然后递归左子树`dfs(root.left, sum - root.val, path)`，右子树`dfs(root.left, sum - root.val, path)`；注意当左右子树都遍历完后，表示以`root`为结点的子树遍历完成了，此时`path`需要回溯到之前的状态，保证遍历`root`的兄弟结点时，`root`已经不在`path`中了。

最后递归结束条件即可。

## 代码

```python
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        self.all_path = []
        self.dfs(root, targetSum, [])
        return self.all_path

    def dfs(self, root, sum_, path):
        if root is None:
            return
        path.append(root.val)
        if root.left is None and root.right is None and sum_ == root.val:
            self.all_path.append(path[:])

        self.dfs(root.left, sum_ - root.val, path)
        self.dfs(root.right, sum_ - root.val, path)
        path.pop()
```