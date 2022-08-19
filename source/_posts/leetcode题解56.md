---
title: leetcode题解56：合并区间
date: 2022-08-02 19:13:23
tags:
    - leetcode
    - 数组
    - 排序
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第56题](https://leetcode-cn.com/problems/merge-intervals/)

<!--more-->

## 分析

首先两个区间能不能合并，就是判断左端点和右端点的大小关系即可；

大致思路也挺简单，首先对区间按左端点从小到大排序，然后新建一个栈存放合并好的区间，可以现将原始区间的第一个元素压入栈中，然后遍历原始区间，如果栈顶的元素与当前区间有重叠，那么就先弹出栈顶元素，将两个区间合并后压入栈中；如果没有重叠，那么就直接将当前区间压入栈中。

还有一种方法支持只在原数组上修改即可，遍历数组，判断当前第`i`个区间和第`i-1`个区间是否可以合并，如果可以合并，那么就将前一个区间的右端点修改为当前区间的右端点，并删掉第`i`个区间，保持`i`不变；如果不能合并，`i++`。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if len(intervals) == 1:
            return intervals
        intervals.sort()
        i = 1
        while i < len(intervals):
            if intervals[i-1][1] >= intervals[i][0]:
                intervals[i-1][1] = max(intervals[i-1][1], intervals[i][1])
                intervals.pop(i)
            else:
                i += 1
        return intervals
```