---
title: leetcode题解85：最大矩形
date: 2022-10-29 11:41:01
tags:
    - leetcode
    - 数组
    - 单调栈
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第85题](https://leetcode.cn/problems/maximal-rectangle/)

## 分析

有了[力扣第84题题解](TODO)，该题简单很多，仔细想想，如果将`1`的个数作为高度，然后求出最大矩形不就可以了，注意每一行往上数连续`1`的个数作为高度，然后对每一行求出最大矩形，最后求出全局最大值即可。

## 代码

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        m, n = len(matrix), len(matrix[0])
        for j in range(n):
            matrix[0][j] = int(matrix[0][j])

        for i in range(1, m):
            for j in range(n):
                if matrix[i][j] == '1':
                    matrix[i][j] = int(matrix[i-1][j]) + 1
                else:
                    matrix[i][j] = 0

        max_ = 0
        for i in range(m):
            max_ = max(self.largestRectangleArea(matrix[i]), max_)
        return max_

    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []
        heights.append(-1)
        max_ = 0
        for i in range(len(heights)):
            while len(stack) > 0 and heights[stack[-1]] > heights[i]:
                j = stack.pop()
                if len(stack) == 0:
                    w = i
                else:
                    w = i - stack[-1] - 1
                max_ = max(max_, w * heights[j])

            stack.append(i)
        return max_
```