---
title: leetcode题解43：字符串相乘
date: 2022-07-10 16:03:02
tags:
    - leetcode
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第43题](https://leetcode.cn/problems/multiply-strings/)
<!--more-->


## 分析
经典大数乘法，回顾我们如何计算乘法，拿`123x45`为例，
```
    123
x    45
--------
     15
    10
    5
    12
    8
   4
--------
    615
   492
--------
   5535
```

总结就是将各位相乘的结果按照一定方式对齐后，直接按位相加即可。假设一个数的长度为`m`，一个数的长度为`n`，那么乘积的长度一定不超过`m+n`，取`i,j`其中`1<=i<=m, 1<=j<=n`，则第一数的第`i`位与第二个数的第`j`位相乘的结果放到`i+j`位（从右往左数），对所有`i,j`进行以上操作然后按位相加，最后依次进位就好。

## 算法

<details open>
<summary>python</summary>
```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == '0' or num2 == '0':
            return '0'
        total_len = len(num1) + len(num2)
        arr = [0]*total_len
        for i in range(len(num1)-1, -1, -1):
            for j in range(len(num2)-1, -1, -1):
                arr[i+j+1] += int(num1[i]) * int(num2[j])

        for i in range(total_len-1, 0, -1):
            arr[i-1] += arr[i] // 10
            arr[i] %= 10

        start = 1 if arr[0] == 0 else 0
        return "".join(map(str, arr[start:]))
```