---
title: leetcode题解132：分割回文串 II
date: 2023-09-23 11:24:33
tags:
    - 动态规划
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第132题](https://leetcode.cn/problems/palindrome-partitioning-ii/)

<!--more-->

## 分析

有了[第131题](https://blog.caoqinping.com/2023/06/11/leetcode题解131)的理解，该题就简单多了，由于是求最小次数，现在假设我们要求`s[0:j+1]`的最小分割次数，而每个子串是否是回文已经可以用动态规划求出，从而我们可以找回所有以索引`j`结尾的回文的长度，不妨设所有回文为`s[k_1:j], s[k_2,j], ...., s[k_n,j]`，那么意思就说在`k_1, k_2,...,k_n`其中任何一个索引都可以分割一次，要求最小分给次数，就是求`s[0:k_1],s[0:k_2],...,s[0:k_n]`中的最小分割次数，然后将该次数+1，就是`s[0:j+1]`的最小分割次数了；显然这个状态依赖前面的某些状态，就是个动态规划问题。

所以伪代码：
```python
min_cut = [-1 for i in range(len(s)+1)]
for i in range(len(s)):
    最小cut = len(s) # 这里只是为了取最大值，使得后面求最小值更方便点
    for j in range(i+1):
        if s[j:i+1] is 回文
          最小cut = min(最小cut, min_cut[j])
    min_cut[i+1] = 最小cut + 1
```

按照上面的逻辑我们需要两次动态规划，第一次用来求解`s[j:i]`是否为回文串，第二次用来求解最小分割次数。其实代码可以继续优化，将二次动态规划写在一起。

合在一起写，由于我们要优先判断后面的回文情况，所以写法有所不同。具体可以看代码。建议仔细与[第131题](https://blog.caoqinping.com/2023/06/11/leetcode题解131/#more)的写法对比，就可以体会其中的差别。

## 代码

```python
class Solution:
    def minCut(self, s: str) -> int:
        dp = [[False for _ in range(len(s))] for _ in range(len(s))]
        min_cut = [-1 for i in range(len(s)+1)]

        for i in range(len(s)):
            min_cut[i+1] = len(s)
            for j in range(i, -1, -1):
                if s[j] == s[i] and (i - j < 2 or dp[j+1][i-1]):
                    dp[j][i] = True
                    min_cut[i+1] = min(min_cut[i+1], min_cut[j] + 1)
        return min_cut[len(s)]
```