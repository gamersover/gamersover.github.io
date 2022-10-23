---
title: leetcode题解72：编辑距离
date: 2022-09-28 14:14:18
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第72题](https://leetcode.cn/problems/edit-distance/)

<!--more-->

## 分析

典型的动态规划题，用`dp[i][j]`表示`word1[:i]`与`word2[:j]`的最小编辑距离，可知`i=0`或`j=0`表示其中一个是空串，而任意字符串与空串`word`之间的编辑距离肯定是`word`的长度，从而`dp[0][j]=j, dp[i][0]=i`，而对于`i>0,j>0`，可以直到`dp[i][j]`必然是一下三种情况的最小值：
1. `dp[i][j-1]+1`
2. `dp[i-1][j]+1`
3. `dp[i-1][j-1]+neq(word1[i-1], word2[j-1])`

其中`neq(a, b)`表示当`a`等于`b`时取`0`，或者取`1`；按照上面的方式就已经可以写代码了，但是其实可以再简化一个转移方程为：
$$
    dp[i][j] = \left
             \{ \begin{aligned}
                    & dp[i-1][j-1]  & \text{word1}[i-1]=\text{word1}[j-1] \\
                    & \min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1 & others
                \end{aligned}
            \right.
$$


## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0 for _ in range(len(word2) + 1)] for _ in range(len(word1) + 1)]
        for i in range(1, len(word1) + 1):
            dp[i][0] = i
        for j in range(1, len(word2) + 1):
            dp[0][j] = j
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
        return dp[-1][-1]
```