---
title: leetcode题解120：三角形最小路径和
date: 2022-11-24 16:05:11
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第120题](https://leetcode.cn/problems/triangle/)

<!--more-->

## 分析

有一种典型的动态规划问题，记`dp[i][j]`为从山顶到达第`i`行第`j`列的最短路径，那么能达到`(i, j)`这个位置的上一个位置只能是`(i-1, j-1)`和`(i-1, j)`，从而转移方程为`dp[i][j] = min(dp[i-1][j], dp[i-1][j-1]) + triangle[i][j]`；

看本题的要求是`O(n)`的额外空间，从而直接使用`triangle`作为状态存储，即`triangle[i][j] += min(triangle[i-1][j], triangle[i-1][j-1])`，注意下边界条件就好；最后取最终层的最小值就好。

一种等价但是代码更加简洁的方法就是从山底向山顶状态转移，这样最终结果就是算完后山顶的值。

## 代码

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        for i in range(1, n):
            for j in range(i, -1, -1):
                if 0 < j < i:
                    triangle[i][j] += min(triangle[i-1][j], triangle[i-1][j-1])
                elif j == 0:
                    triangle[i][j] += triangle[i-1][j]
                else:
                    triangle[i][j] += triangle[i-1][j-1]
        return min(triangle[n-1])
```