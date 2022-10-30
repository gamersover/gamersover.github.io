---
title: leetcode题解90：子集 II
date: 2022-10-30 10:46:21
tags:
    - leetcode
    - 数组
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第90题](https://leetcode.cn/problems/subsets-ii/)

## 分析

和[力扣第78题题解](TODO)几乎一样，多了一个重复元素的处理，模拟下生成过程，就知道重复元素如何处理了，以`[1,2,2]`为例，
1. 空集，`[]`
2. 添加`1`，`[], [1]`
3. `2`与`1`不同，继续添加`[], [1], [2], [1, 2]`
4. 最后一个`2`与前面`2`相同，这时如果依然在上一步中的每个子集都添加就重复了，因为`[], [1]`已经加过`2`了，所以只需要对上一步新生成的子集添加`2`就可以，即`[2,2], [1,2,2]`

从上面的过程可以看出，对于重复元素的添加区别仅在于，需要添加元素的子集的起点；新元素直接从`0`开始，如果是重复的元素，从上一步添加的新元素位置开始，即上上步的结束位置。

## 代码

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = [[]]
        start = len(res)
        res.append(res[0] + [nums[0]])
        end = len(res)
        for i in range(1, len(nums)):
            s = start if nums[i] == nums[i-1] else 0
            for j in range(s, end):
                res.append(res[j] + [nums[i]])
            start = end
            end = len(res)
        return res
```