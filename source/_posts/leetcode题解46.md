---
title: leetcode题解46：全排列
date: 2022-07-16 12:19:01
tags:
    - leetcode
    - 数组
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第46题](https://leetcode.cn/problems/permutations/)
<!--more-->

## 分析

子问题分解，比如说要排列`1 2 3`，那就先取`1`，全排列`2 3`，得到`1 2 3`和`1 3 2`，依次下去，直到取完所有的数字。相当于全排列`n`个数，可以先求`n-1`个数的全排列，分解成一个子问题，显然用递归求解非常简单。

至于代码上的编写技巧，非`python`，取第`i`个数时，可以先与第`0`个数交换，然后排列数组的后`n-1`位，最后再交换回来就可以了。

## 代码

<details open>
<summary>python</summary>
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        if len(nums) == 1:
            return [nums]
        res = []
        for i in range(len(nums)):
            for subres in self.permute(nums[:i] + nums[i+1:]):
                res.append([nums[i]] + subres)
        return res
```