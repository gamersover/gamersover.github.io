---
title: leetcode题解134：加油站
date: 2023-09-24 12:35:15
tags:
    - 数组
    - 双指针法
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第134题](https://leetcode.cn/problems/gas-station)

<!--more-->

## 分析

这题很有意思，一定要注意到题目说明：**如果存在解，则保证它是唯一的。** 解只有一个。

首先我们可以计算出每一站相对消耗量，即`delta[i] = gas[i]-cost[i]`，我们可以主要观察`delta`数组.

1. 首先可以注意到，有解的必要条件是`delta`数组的和大于0；

2. 然后我们可以对delta数组从索引`i`开始求和，如果
$$
    \sum_{k=i}^j delta[k] >= 0
$$
表示从`i`到`j`汽油站都是安全的；一旦在某个`l`汽油站**第一次**出现
$$
    \sum_{k=i}^l delta[k] < 0
$$
表示从`i`开始出发就不安全了，其实不仅从`i`开始，而是从`i`一直到`l`任何一个加油站`l_1`出发都不安全。因为
$$
    \sum_{k=l_1}^l delta[k] = \sum_{k=i}^l delta[k] -  \sum_{k_i}^{l_1} delta[k] < 0
$$
3. 注意到1可以让我们判断有没有解，2可以让我们排除非解，最后一个就是让我们知道解在哪里。那就是2的另一种情况，没有遇到小于0的汽油站，即从`i`开始的和
$$
    \sum_{k=i}^{j} delta[i] >= 0
$$
其中$i \le j \le len(gas)$。那么最终结果一定是`i`，为什么？这就要用到解只有一个的假设了，我们用反证法，如果$i+\delta$才是那个解，又由于$i$到$i+\delta$都是安全的，那么从$i$开始更是安全的，所以$i$也是那个解，从而有多解了，不符合题意。


## 代码

```python
def canCompleteCircuit(self, gas, cost):
    start, total, curr = 0, 0, 0
    for i in range(len(gas)):
        delta = gas[i] - cost[i]
        total += delta
        curr += delta
        if curr < 0:
            start = i + 1
            curr = 0

    return -1 if total < 0 else start
```