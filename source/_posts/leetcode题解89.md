---
title: leetcode题解89：格雷编码
date: 2022-10-30 10:37:38
tags:
    - leetcode
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第89题](https://leetcode.cn/problems/gray-code/)

## 分析

首先假设长度为`2`的格雷码已经生成好了，为
```
00 01 11 10
```
现在要生成长度为`3`的格雷码，直接将长度为`2`的翻转并追加到后面，
```
00 01 11 10 10 11 01 00
```
可以此时每个字符相差不超过`1`，这时前半部分添加前缀`0`，后半部分添加前缀`1`，就好了
```
000 001 011 010 110 111 101 100
```

用这种递归方法就可以生成任意长度的格雷码。

还有一种方法直接使用公式生成，第`n`个格雷码为`n ^ (n >> 1)`。


## 代码

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        # 递归方法
        if n == 1:
            return [0, 1]
        pre_code = self.grayCode(n-1)
        new_code = pre_code[:]
        for p in pre_code[::-1]:
            new_code.append(p + (1 << (n-1)))
        return new_code

    def grayCode2(self, n):
        # 公式生成法
        res = []
        for i in range(1 << n):
            res.append(i ^ (i >> 1))
        return res
```