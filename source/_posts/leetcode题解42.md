---
title: leetcode题解42：接雨水
date: 2022-05-21 11:59:39
tags:
    - leetcode
    - 数组
    - 动态规划
categories: 算法
mathjax: true
---


## 描述

该题来自于[力扣第42题](https://leetcode.cn/problems/trapping-rain-water/)
<!--more-->


## 分析

最直接的想法就是求出哪两个柱子之间可以接雨水，然后算出每两个柱子之间能够接的雨水量；但是效率不高，仔细分析下计算过程，现假设编号`i`到`i+k-1`这`k`个柱子之间可以接雨水，那么有两个性质：
1. 雨水的最大高度是两个边界柱子的最小高度，记`h=min(arr[i], arr[i+k-1])`，`h`也是每根柱子接雨水的高度
2. 接的雨水的单位是`h*k - (arr[i] + arr[i+1] + arr[i+k-1])`，分解下得到`(h - arr[i]) + (h - arr[i+1]) + (h - arr[i+k-1])`，发现能接的雨水的总单位就是每跟柱子能接雨水的高度减去自身的高度，然后求和即可

性质2说明能接雨水总单位等于 每跟柱子能接的雨水单位的总和，而性质1说明，每根柱子接雨水的高度就是，自身左边最大高度与自身右边最大高度的最小值。举个例子：

> 1 0 <font color="blue">2</font> 1 <font color="red">0</font> <font color="blue">1</font>

红色0左边的最大值为2，右边最大值为1，从而红色1接雨水的高度为1，而自身高度为`0`，所以该柱子能接雨水的单位是`1-0=1`；对于每个柱子接雨水的高度可以这样求解：先求出每个柱子的左边最大高度，再求出每个柱子的右边高度，然后求两个最小值即可。设编号为`i`的柱子左边最大高度为`f(i)`，显然有`f(i) = max(f(i-1), arr[i])`，所以在求解时可以使用动态规划。


## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left = [height[0]]*len(height)
        for i in range(1, len(height)):
            left[i] = max(left[i-1], height[i])

        right = [height[-1]]*len(height)
        for i in range(len(height)-2, -1, -1):
            right[i] = max(right[i+1], height[i])

        area = 0
        for i in range(len(height)):
            area += min(left[i], right[i]) - height[i]
        return area
```
</details>
