---
title: leetcode题解54：螺旋矩阵
date: 2022-08-02 18:57:30
tags:
    - leetcode
    - 数组
    - 模拟
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第54题](https://leetcode-cn.com/problems/spiral-matrix/)
<!--more-->

## 分析

该题思路很简单，就是模拟螺旋遍历的过程，首先遍历最外面一圈，然后缩小一层，继续遍历这一圈，直到遍历完所有圈。遍历的总层数由矩阵的行数和列数的最小值的一半决定。确定层数后，计算出每一圈的四个角位置，然后按照顺序遍历即可。

## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        m = len(matrix)
        n = len(matrix[0])
        max_layer = (min(m, n) + 1) // 2
        ans = []
        for layer in range(max_layer):
            for i in range(layer, n-layer):
                ans.append(matrix[layer][i])
            for i in range(layer+1, m-layer):
                ans.append(matrix[i][n-layer-1])
            if len(ans) == n * m:
                return ans
            for i in range(n-layer-2, layer-1, -1):
                ans.append(matrix[m-layer-1][i])
            for i in range(m-layer-2, layer, -1):
                ans.append(matrix[i][layer])
        return ans
```

