---
title: leetcode题解91：解码方法
date: 2022-11-05 11:02:05
tags:
    - leetcode
    - 字符串
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第91题](https://leetcode.cn/problems/decode-ways/)

<!--more-->

## 分析

假设`s`为$a_0a_1...a_{n-2}a_{n-1}a_n$，由于每个字母最多转为2位的字符串，那么从后面往前看，一种是$a_n$单独解码，一种是$a_{n-1}a_n$一起解码，所以如果是$a_n$单独解码，那么只需知道$a_0a_1...a_{n-1}$有多少中解码方式；如果是$a_{n-1}a_n$一起解码，那么只需知道$a_0a_1...a_{n-2}$有多少中解码方式；最后这两种解码方式相加即可。

所以这是个动态规划问题，记`dp[i]`表示从$a_0...a_i$的所有解码方式，$a_i$可以单独解码的充分必要条件是$a_i \ne 0$，`0`字符串不能转码；而$a_{i-1}a_i$可以转码的充分必要条件是$10 \le a_{i-1}a_{i} \le 26$，满足两个条件则
```
dp[i] = dp[i-1] + dp[i-2]
```
只满足其中一个，另一个就不加到结果上即可，最后注意边界条件就好。

## 代码

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        dp = [0]*len(s)
        for i in range(len(s)):
            if s[i] != '0':
                dp[i] += dp[i-1] if i > 0 else 1
            if i > 0 and '10' <= s[i-1:i+1] <= '26':
                dp[i] += dp[i-2] if i > 1 else 1
        return dp[len(s)-1]
```