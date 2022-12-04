---
title: leetcode题解95：不同的二叉搜索树 II
date: 2022-11-05 11:35:12
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第95题](https://leetcode.cn/problems/unique-binary-search-trees-ii/)

<!--more-->

## 分析

关于树的问题，基本上可以靠递归解决，即大问题转换为小问题。比如用`1 2 3 4 5`构建二叉搜索树，由于是搜索树，左子树结点都小于根结点，右子树结点都大于根结点，比如选`3`为根结点，则用`1 2`构建二叉搜索树作为左子树，`4 5`构建的二叉树作为右子树，这就是相似的“小问题”了。

所以使用递归，遍历所有结点作为根结点，然后左边的数用来构建左子树，右边的数用来构建右子树，所有可能的左子树与右子树结合即可。

## 代码

```python
class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        return self.generate(1, n+1)

    def generate(self, start, end):
        if start == end:
            return [None]
        if start == end - 1:
            return [TreeNode(start)]
        ans = []
        for i in range(start, end):
            left_trees = self.generate(start, i)
            right_trees = self.generate(i+1, end)
            for l in left_trees:
                for r in right_trees:
                    head = TreeNode(i)
                    head.left = l
                    head.right = r
                    ans.append(head)
        return ans
```