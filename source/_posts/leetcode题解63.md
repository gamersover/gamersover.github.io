---
title: leetcode题解63：不同路径 II
date: 2022-08-06 13:43:28
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第63题](https://leetcode-cn.com/problems/unique-paths-ii/)

<!--more-->

## 分析

与[力扣第62题](https://caoqinping.com/2022/08/06/leetcode题解62/)唯一不同的地方，在于有障碍物，不难发现，到达`i,j`网格的步数，依然是相邻左方与上方的步数之和，不过如果有一个相邻的区域是障碍物，那么该方向就失效，那么设置达到障碍物的步数为`0`就可以。所以转移方程为
$$
    dp[i][j] = \left\{
        \begin{aligned}
            & 0 & i,j\text{是障碍物}\\
            & dp[i][j-1] & i,j\text{不是障碍物且 } i == 0 \text{ and } j > 0 \\
            & dp[i-1][j] & i,j\text{不是障碍物且 } j == 0 \text{ and } i > 0 \\
            & dp[i-1][j] + dp[i][j-1] & others
        \end{aligned}
    \right.
$$
注意初始化`dp[0][0]=1`

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if obstacleGrid[i][j] == 0:
                    if i == 0 and j == 0:
                        dp[i][j] = 1
                        continue
                    if i > 0:
                        dp[i][j] += dp[i-1][j]
                    if j > 0:
                        dp[i][j] += dp[i][j-1]
        return dp[m-1][n-1]
```
