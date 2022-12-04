---
title: leetcode题解115：不同的子序列
date: 2022-11-24 14:56:54
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第115题](https://leetcode.cn/problems/distinct-subsequences/)

<!--more-->

## 分析

这种问题应该首先想到动态规划是否可以解决，以`dp[i][j]`表示`s[:i]`的子序列中`t[:j]`出现的个数，然后看看转移方程是什么？

当`s[i-1] = t[j-1]`时，有两种情况：1. 不算上`s[i-1]`，即`s[:i-1]`的子序列中`t[:j]`出现的个数`dp[i-1][j]`；2. 算上`s[i-1]`，即`s[:i-1]`中的子序列中`t[:j-1]`出现的个数`dp[i-1][j-1]`；两种情况相加。

否则就是`s[:i-1]`的子序列中`t[:j]`出现的个数`dp[i-1][j]`。

所以转移方程为
$$
    dp[i][j] = \left\{
        \begin{aligned}
            & dp[i-1][j] + dp[i-1][j-1] & s[i-1]=t[j-1]\\
            & dp[i-1][j] & others
        \end{aligned}
    \right.
$$

可以看到`dp[i][?]`只与`dp[i-1][?]`状态有关，从而可以状态压缩，使用一维数组表示，所以
```
    dp[k] = dp[k] + dp[k-1] * (s[i-1] == t[j-1]) ? 1 : 0
```
又由于`dp[k]`需要使用`dp[k-1]`的状态，所以`k`从后面开始更新，而初始状态`dp[0]=1`，其他设为`0`。


## 代码

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        if len(s) < len(t):
            return 0

        dp = [0 for _ in range(len(t)+1)]
        dp[0] = 1
        for i in range(1, len(s)+1):
            for j in range(len(t), 0, -1):
                if s[i-1] == t[j-1]:
                    dp[j] += dp[j-1]
        return dp[len(t)]
```