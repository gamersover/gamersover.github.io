---
title: leetcode题解47：全排列 II
date: 2022-07-16 12:25:27
tags:
    - leetcode
    - 数组
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第47题](https://leetcode.cn/problems/permutations-ii/)
<!--more-->

## 分析

类似[第40题](#)对于[第30题](#)的改变一样，每次遍历的时候，注意跳过重复数字就好，至于跳过重复数组的方法，有很多种；比如先对数组排序，然后判断前后两个数字是否一样，一样就跳过；又或者初始化一个集合，然后判断集合中是否包含遍历到的数字，包含就跳过。

## 代码

<details open>
<summary>python</summary>
```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums = sorted(nums)
        if len(nums) == 1:
            return [nums]
        res = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            for subres in self.permuteUnique(nums[:i] + nums[i+1:]):
                res.append([nums[i]] + subres)
        return res
```
