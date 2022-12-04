---
title: leetcode题解96：不同的二叉搜索树
date: 2022-11-05 11:42:25
tags:
    - leetcode
    - 二叉树
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第96题](https://leetcode.cn/problems/unique-binary-search-trees/)

<!--more-->

## 分析

与[力扣第95题](https://caoqinping.com/2022/11/05/leetcode题解95/)不同之处在于只需要求个数，不需要求具体树结构，虽然可以直接用第95题题解，加个返回长度即可；但是只是求个数，其实可以用动态规划。

因为是大问题化解成小问题，可以发现树的构成个数之和输入数组长度有关，所以如果用`dp[i]`表示长度为`i`的数组构成的搜索二叉树的个数，那么一个长度为`n`的数组，选定一个根结点`k`时，左边还有`k-1`个数，右边还有`n-k`个数，从而左子树有`dp[k-1]`种，右子树有`dp[n-k]`种，从而`dp[k-1]*dp[n-k]`就是根节点为`k`时的所有树的个数。

从而`dp[n]`依然于前面所有的状态，转移方程就是
$$
    dp[n] = \sum_{i=1}^n dp[i-1]*dp[n-i]
$$

## 代码

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0 for i in range(n+1)]
        dp[0] = 1
        dp[1] = 1
        for k in range(2, n+1):
            for i in range(k):
                dp[k] += dp[i]*dp[k-i-1]
        return dp[n]
```