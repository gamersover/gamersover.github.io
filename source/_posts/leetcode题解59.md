---
title: leetcode题解59：螺旋矩阵 II
date: 2022-08-03 13:46:11
tags:
    - leetcode
    - 数组
---

## 描述

该题来自于[力扣第59题](https://leetcode-cn.com/problems/spiral-matrix-ii/)

<!--more-->

## 分析

该题与[力扣第54题](https://caoqinping.com/2022/08/02/leetcode题解54/)类似，依然是按照层数遍历，遍历到的位置将值赋给数组即可。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        max_layer = (n + 1) // 2
        ans = [[0]*n for _ in range(n)]
        number = 1
        for layer in range(max_layer):
            for i in range(layer, n-layer):
                ans[layer][i] = number
                number += 1
            for i in range(layer+1, n-layer):
                ans[i][n-layer-1] = number
                number += 1
            if len(ans) == n *n:
                return ans
            for i in range(n-layer-2, layer-1, -1):
                ans[n-layer-1][i] = number
                number += 1
            for i in range(n-layer-2, layer, -1):
                ans[i][layer] = number
                number += 1
        return ans
```
