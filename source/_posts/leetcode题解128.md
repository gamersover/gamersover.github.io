---
title: leetcode题解128：最长连续序列
date: 2023-02-05 11:09:29
tags:
    - leetcode
    - 数组
    - 哈希表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第128题](https://leetcode.cn/problems/longest-consecutive-sequence/)

<!--more-->

## 分析

只能`O(n)`的时间复杂度，那么必须记住已经遍历过的数所在的连续区间的长度，仔细思考下：如果已知前面的数所在的连续区间的长度，那么当加入一个没出现过的数时，该数只能是之前连续区间的左端点或右端点，又或者根本无法构成连续区间。不可能是中点某个点的原因在于该数之前没有出现过。

如果用一个字典`d`记录以遍历过数所在连续区间的长度，那么当遍历到未出现过的数`n`时，如果与之前构成连续区间，可知只能是左端点或右端点，那么取出`d[n-1]`和`d[n+1]`，如果`d[n-1]`存在则表示新数是某个连续区间的右端点，如果`d[n+1]`存在则表示新数是某个新数的左端点，最后更新`d[n]=d[n-1]+d[n+1]+1`；注意还是由于新数只会是某个连续区间左端点或右端点，从而新连续区间的左端点`n-d[n-1]`和右端点`n+d[n+1]`的所在区间的长度也需要更新，即`d[n - d[n-1]]`和`d[n + d[n+1]]`也要通过更新。


## 代码

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        dp = dict()
        ans = 0
        for n in nums:
            if n not in dp:
                left = dp.get(n-1, 0)
                right = dp.get(n+1, 0)
                curr_len = left + right + 1
                if curr_len > ans:
                    ans = curr_len
                dp[n] = curr_len
                dp[n-left] = curr_len
                dp[n+right] = curr_len
        return ans
```