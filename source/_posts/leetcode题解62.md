---
title: leetcode题解62：不同路径
date: 2022-08-06 13:36:19
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第62题](https://leetcode-cn.com/problems/unique-paths/)

<!--more-->

## 分析

最经典也最基础的动态规划题了，用`dp[i][j]`表示到达`i,j`这个网格的步数，这有转移方程
$$
    dp[i][j] = \left\{ \begin{aligned} & 1 & i == 0\text{ or }j == 0 \\ & dp[i-1][j] + dp[i][j-1] & others \end{aligned}\right.
$$

即步数总是等于到达相邻的上方与左方的步数之和。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if i == 0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[m-1][n-1]
```


