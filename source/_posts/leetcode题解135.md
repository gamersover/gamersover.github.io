---
title: leetcode题解135：分发糖果
date: 2023-09-24 13:35:07
tags:
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第135题](https://leetcode.cn/problems/candy)

<!--more-->

## 分析

这题虽然是困难难度，其实很简单，评分有四种情况（简化后实际是2种），一一来看；
1. 递增，如 `3 5 7`，直接取 `1 2 3`
2. 递减，如 `7 5 3`，反过来来看就是递增，一样处理
3. 先递增后递减，`1 3 5 3 2 1`，这种情况关键在于转折点，分开看就是前两种情况，`1 3 5` 和 `5 3 2 1`，其中`5`是转折点，除了转折点不一样，其他都一样，先取为 `1 2 x 3 2 1`，那转折点`x`应该取什么呢？显然取最大`4`；
4. 先递减后递增；其实反过来就是第3点，也是一样的处理方法。

总结下就是先从左往右处理递增取值，递减直接取1；然后从右往左递增取值，递减取1，最后取两个的最大值即可。代码优化时，其实只要转折点取两者最大，非转折点不必处理，

## 代码

```python
from typing import List


class Solution:
    def candy(self, ratings: List[int]) -> int:
        candy_arr = [1 for i in range(len(ratings))]
        for i in range(1, len(ratings)):
            if ratings[i] > ratings[i-1]:
                candy_arr[i] = candy_arr[i-1] + 1
        for i in range(len(ratings)-1, 0, -1):
            if ratings[i] < ratings[i-1] and candy_arr[i] >= candy_arr[i-1]:
                candy_arr[i-1] = candy_arr[i] + 1
        return sum(candy_arr)
```
