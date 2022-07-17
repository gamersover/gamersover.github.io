---
title: leetcode题解44：通配符匹配
date: 2022-07-10 16:56:36
tags:
    - leetcode
    - 字符串
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第44题](https://leetcode.cn/problems/wildcard-matching/)
<!--more-->

## 分析

类似于[leetcode题解10](https://caoqinping.com/2020/12/05/leetcode%E9%A2%98%E8%A7%A310/)，同样使用动态规划来解，定义`dp[i][j]`为字符串`s`的前`i`位与字符串`p`的前`j`为匹配的结果，则`dp[0][0]`表示两个空串的比较，自然为`True`；对于`i>=1`和`j>=1`不难知道状态转移方程为
$$
    dp[i][j] = \left\{
        \begin{aligned} & dp[i-1][j-1] & p[j-1]=?  \\
                        & dp[i-1][j] \quad \text{or} \quad dp[i][j-1] & p[j-1]=* \\
                        & dp[i-1][j-1] \quad \text{and} \quad s[i-1][j-1] & \text{others}
        \end{aligned} \right.
$$

最后注意边界，主要是`dp[0][j]`这种情况。

## 代码

<details open>
<summary>python</summary>
```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        dp = [[False for j in range(len(p)+1)] for i in range(len(s)+1)]
        dp[0][0] = True
        for j in range(1, len(p) + 1):
            dp[0][j] = dp[0][j-1] and p[j-1] == '*'

        for i in range(1, len(s)+1):
            for j in range(1, len(p)+1):
                if p[j-1] == '?':
                    dp[i][j] = dp[i-1][j-1]
                elif p[j-1] == '*':
                    dp[i][j] = dp[i-1][j] or dp[i][j-1]
                else:
                    dp[i][j] = dp[i-1][j-1] and s[i-1]== p[j-1]
        return dp[len(s)][len(p)]
```
