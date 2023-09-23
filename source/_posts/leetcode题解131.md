---
title: leetcode题解131：分割回文串
date: 2023-06-11 11:46:58
tags:
    - 动态规划
    - 递归
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第131题](https://leetcode.cn/problems/palindrome-partitioning/)

<!--more-->

## 分析

需要列举出所有分割方案，显然要用到递归了，先找出前缀回文串，然后分割后面的子串就好了，这样大问题又转为小问题了。

假设原始字符串为`s`，求解该问题的函数是`f(s, start)`，显然返回值是一个二维数组；给出起始索引`start`，遍历索引`i`，那么如果`s[start:i+1]`是回文串，继续求解`f(s, i+1)`，得到的结果就是从`i`开始的子串的所有分割方案。从而遍历这个结果，并将前缀回文串`s[start:i+1]`添加到每个方案数组的头部就好了，伪代码如下：
```python
ans = []
for i in range(start, len(s)):
    pre = s[start:i+1]
    if pre is 回文:
        for ans_ in f(s, i+1):
            ans.append([pre] + ans_)
```

所以关键在于先求出`s[i:j]`是否是回文串，这要这个可以判断，那么上面的方法就可以实现。

当然直接暴力求解没有问题，但是考虑到我们需要求解出所有`i <= j`时`s[i:j]`是否为回文串的情况，暴力时间复杂度太高，而且显然可以用动态规划优化；如果我们知道`s[i:j]`，那么求解`s[i-1:j+1]`就很简单，判断`s[i-1]==s[j+1]`就好，根据这种思想，判断是否为回文串的伪代码如下：
```python
dp = [[True for _ in range(len(s))] for _ in range(len(s))]
for i in range(1, len(s)):
    for j in range(i+1):
        dp[j][i] = (s[i] == s[j]) and ((i - j <= 2) or dp[j+1][i-1])
```


## 代码

```python
from typing import List


class Solution:
    def partition(self, s: str) -> List[List[str]]:

        dp = [[True for _ in range(len(s))] for _ in range(len(s))]
        for i in range(1, len(s)):
            for j in range(i+1):
                dp[j][i] = (s[i] == s[j]) and ((i - j <= 2) or dp[j+1][i-1])

        def split(s, start):
            if start == len(s):
                return [[]]
            ans = []
            for i in range(start, len(s)):
                if dp[start][i]:
                    pre = [s[start:i+1]]
                    for sub in split(s, i+1):
                        ans.append(pre + sub)
            return ans

        return split(s, 0)
```