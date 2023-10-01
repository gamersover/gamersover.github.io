---
title: leetcode题解139：单词拆分
date: 2023-10-01 11:27:36
tags:
    - 动态规划
    - 字符串
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第139题](https://leetcode.cn/problems/word-break)

<!--more-->

## 分析

像这种不需要求所有分割点，所有解的，而是求存不存在或者最小最大值的一般用动态规划，否则就用递归。

定义状态`dp[i+1]`表示`s[0:i+1]`子串能不能拼接出来，如果`dp[i+1]=True`，那么一定存在一个`0 < j <= i`，使得`dp[j]=True`，而且`s[j:i+1]`一定在`wordDict`中。反之，如果不存在，那表示`s[0:i+1]`无法拼接出来，即`dp[i+1]=False`。

## 代码

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False for i in range(len(s)+1)]
        dp[0] = True
        word_set = set(wordDict)
        for i in range(len(s)):
            for j in range(i+1):
                if dp[j] and s[j:i+1] in word_set:
                    dp[i+1] = True
                    break
        return dp[len(s)]
```
