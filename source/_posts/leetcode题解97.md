---
title: leetcode题解97：交错字符串
date: 2022-11-05 11:55:16
tags:
    - leetcode
    - 字符串
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第97题](https://leetcode.cn/problems/interleaving-string/)

<!--more-->

## 分析

比如字符串$s_1=a_1a_2..a_l$，$s_2=b_1b_2..b_m$，$s_3=c_1c_2...c_{l+m}$，要判断$s_3$是否由$s_1,s_2$交错生成，还是以化成小问题为目标，首先当$a_l$与$b_m$都不等于$c_{l+m}$时，显然不可能是$s_1,s_2$生成的；

从而先假设$a_l = c_{l+m}$，那么不就是看$a_1...a_{l-1}$与$s_2$是否可以生成$c_1...c_{l+m-1}$吗？这就化成了相似的小问题了。

$b_m = c_{l+m}$时，同理可得。所以该题可以用动态规划解决。记`dp[i][j]`表示$a_1...a_i$与$b_1...b_j$是否可生成$c_1...c_{i+j}$，那么转移方程就是
```
    dp[i][j] = (dp[i-1][j] & s1[i]==s3[i+j]) | (dp[i][j-1] & s2[j]==s3[i+j])
```

最后注意边界条件即可。

## 代码

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if (len(s1) + len(s2)) != len(s3):
            return False

        dp = [[False for _ in range(len(s2)+1)] for _ in range(len(s1)+1)]
        dp[0][0] = True
        for i in range(0, len(s1)+1):
            for j in range(0, len(s2)+1):
                if i > 0:
                    dp[i][j] = (s1[i-1] == s3[i+j-1]) and dp[i-1][j]
                if j > 0:
                    dp[i][j] |= (s2[j-1] == s3[i+j-1] and dp[i][j-1])
        return dp[len(s1)][len(s2)]
```