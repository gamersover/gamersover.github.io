---
title: leetcode题解105：从前序与中序遍历序列构造二叉树
date: 2022-11-06 10:33:04
tags:
    - leetcode
    - 二叉树
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第99题](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

<!--more-->

## 分析

假设先序遍历是`3 9 20 15 7`，中序遍历是`9 3 15 20 7`，那么显然`3`是根结点，有了根结点看中序遍历知道`9`是左子树，`15 20 7`是右子树，从而左子树的先序遍历和中序遍历分别为`9`, `9`，而右子树的先序遍历和中序遍历分别为`20 15 7`，`15 20 7`；所以大问题又化解成了小问题，用递归就可以轻松解决。

具体步骤：
1. 先将先序遍历中的第一个结点作为根结点，然后找出其在中序遍历中的位置，从而获取左子树的长度，右子树的长度
2. 再从总的先序遍历和中序遍历中获得左子树和右子树的先序遍历和中序遍历，递归构建


## 代码

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if len(preorder) == 0:
            return None
        if len(preorder) == 1:
            return TreeNode(preorder[0])

        len_ = 0
        for i in range(len(inorder)):
            if inorder[i] == preorder[0]:
                break
            len_ += 1

        left = self.buildTree(preorder[1:1+len_], inorder[:len_])
        right = self.buildTree(preorder[1+len_:], inorder[1+len_:])
        return TreeNode(preorder[0], left, right)
```