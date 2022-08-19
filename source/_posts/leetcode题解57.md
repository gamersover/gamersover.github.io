---
title: leetcode题解57：插入区间
date: 2022-08-02 19:44:30
tags:
    - leetcode
    - 数组
    - 排序
categories: 算法
mathjax: true
---


## 描述

该题来自于[力扣第57题](https://leetcode-cn.com/problems/insert-interval/)

<!--more-->

## 分析

这里最简单的思路就是找到区间插入的位置，然后利用上一题的思路合并区间即可。但是发现写起来不是那么方便；

本质是找到需要合并的起始区间和结束区间，当没有到达起始区间时，什么都不需要改，直接将当前区间加入到结果中；当到达起始区间时，开始合并，直到最后一个需要合并的区间结束，并将合并后的区间加入到结果中，然后将剩余的区间加入到结果中即可。也就是说只要加入一个标志位，标识是否到达的结束区间，即合并结束。

这里合并区间时，有个小优化，就是记录并更新区间的左端点和右端点即可。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        left, right = newInterval
        ans = []
        placed = False
        for first, second in intervals:
            if first > right:
                if not placed:
                    ans.append([left, right])
                    placed = True
                ans.append([first, second])
            elif second < left:
                ans.append([first, second])
            else:
                left = min(left, first)
                right = max(right, second)

        if not placed:
            ans.append([left, right])
        return ans
```