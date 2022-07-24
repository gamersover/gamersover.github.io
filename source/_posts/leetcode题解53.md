---
title: leetcode题解53
date: 2022-07-24 13:20:17
tags:
    - leetcode
    - 数组
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第53题](https://leetcode.cn/problems/maximum-subarray/)
<!--more-->

## 分析

先说一种相对直观的思路：初始化当前和为0，遍历数组，求当前和，与全局最大和比较大小，并更新全局最大和；如果当前和小于0，则表示包含当前值得子数组的和一定不可能是最大值，所以当前和重置为0。

另一种等价的思路是动态规划，记`dp[i]`是以第`i`个数结尾的子数组的最大和，那么考虑以第`i+1`个数结尾的子数组的最大和，以第`i+1`个数结尾的子数组的最大和有两种情况，一种是`dp[i]+nums[i+1]`，一种是`nums[i+1]`；从而`dp[i+1]=max(nums[i], dp[i]+nums[i+1])`。然后再求`dp`数组的最大值就可以，这里也可以用全局最大来更新。这里可以看到`dp[i+1]`只与`dp[i]`有关，所以可以压缩数组，用一个数表示就好了。

## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        ans, curr = nums[0], nums[0]
        for i in range(1, len(nums)):
            curr = max(nums[i], curr+nums[i])
            ans = max(ans, curr)
        return ans
```
