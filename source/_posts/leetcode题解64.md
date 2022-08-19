---
title: leetcode题解64：最小路径和
date: 2022-08-06 14:03:00
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第64题](https://leetcode-cn.com/problems/minimum-path-sum/)

<!--more-->

## 分析

和[力扣第62题](https://caoqinping.com/2022/08/06/leetcode题解62/)的区别在于，试求最小和，还是用`dp[i][j]`表示达到`i,j`网格的最小和，那么转移方程为`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`，当然这是对`i>0，j>0`的情况，`i==0`或`j==0`的情况，就更简单了，具体可以参考代码。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        for i in range(1, m):
            grid[i][0] += grid[i-1][0]
        for j in range(1, n):
            grid[0][j] += grid[0][j-1]

        for i in range(1, m):
            for j in range(1, n):
                grid[i][j] += min(grid[i-1][j], grid[i][j-1])
        return grid[m-1][n-1]
```

