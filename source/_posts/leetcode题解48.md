---
title: leetcode题解48：旋转图像
date: 2022-07-23 14:44:03
tags:
    - leetcode
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第48题](https://leetcode.cn/problems/rotate-image/)
<!--more-->

## 分析

通过观察示例可以发现，本质是$(i,j)$替换到$(j, n-i-1)$，如$(0,0)\rightarrow(0,n-1) \rightarrow (n-1, n-1) \rightarrow (n-1, 0) \rightarrow(0, 0)$；

以示例2为例，$5,11,16,15$构成一个循环替换，之后是$1,10,12,13$，然后$9, 7, 14, 2$， 这样最外面一圈就替换完成了，然后缩小一圈重复上面操作就好。

## 算法
1. 遍历所有层layer，从第0层遍历到$n // 2$层就好；
2. 利用上面的替换公式开始循环替换，`matrix[layer][i] -> matrix[i][n-layer-1]->matrix[n-layer-1][n-i-1]->matrix[n-i-1][layer]->matrix[layer][i]`；其中`i`从`layer`到`n-layer-2`；

## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        n = len(matrix)
        for layer in range(n // 2):
            for i in range(layer, n-layer-1):
                t = matrix[layer][i]
                matrix[layer][i] = matrix[n-i-1][layer]
                matrix[n-i-1][layer] = matrix[n-layer-1][n-i-1]
                matrix[n-layer-1][n-i-1] = matrix[i][n-layer-1]
                matrix[i][n-layer-1] = t
```
