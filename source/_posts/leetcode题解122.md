---
title: leetcode题解122：买卖股票的最佳时机 II
date: 2022-12-04 11:43:21
tags:
    - leetcode
    - 动态规划
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第122题](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

<!--more-->

## 分析

股票问题有很多遍历，没有通用的解决思路的话确实有点难度，类似背包问题，考虑动态规划，状态是第`i`天是否持有股票的最大收益，如`dp[i][0]`表示第`i`天不持有股票的最大收益，`dp[i][1]`表示第`i`天持有股票的最大收益，接下来主要分析状态转移过程：

1. 第`i`天不持有，有两种情况：（a）第`i-1`天不持有，（b）第`i-1`天持有，表示第`i`天卖出股票，从而收益增加`prices[i]`；从而`dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`；
2. 第`i`天持有，也有两种情况：（a）第`i-1`天持有，（b）第`i-1`天不持有，表示第`i-1`天买入，从而收益减少`prices[i]`；从而`dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])`；

上述转移方程非常重要，是解所有这种类型题的关键，比如该题的解决方法，可以看到状态只依赖上次的状态，从而设`dp0`为当天不持有股票的最大收益，`dp1`为当天持有股票的最大收益，从而转移方程为
```python
    new_dp0 = max(dp0, dp1 + prices[i])
    new_dp1 = max(dp1, dp0 - prices[i])
    dp0, dp1 = new_dp0, new_dp1
```

而边界条件可以直接考虑，第`0`天不持有显然收益为`0`，从而`dp0 = 0`，而持有收益显然为`-prices[0]`；最终结果显然是最后一天不持有股票的收益最大即`dp1`。


## 代码

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp0 = 0
        dp1 = -prices[0]
        for i in range(1, len(prices)):
            new_dp0 = max(dp0, dp1 + prices[i])
            new_dp1 = max(dp1, dp0 - prices[i])
            dp0, dp1 = new_dp0, new_dp1
        return dp0
```

## 附录：通用解法

如果加上条件，最多只能交易`k`次，求最大收益，那么用`dp[i][k][0],dp[i][k][1]`分别表示在`i`天交易了`k`次不持有和持有的最大收益，由于最多只能持有一支股票，从而买入卖出操作必须一起，为了方便，将买入作为交易次数的记录，从而转移方程为
```python
    dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
    dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])  # 由于买入算交易依次，从而是k-1次不持有的状态
```

而这一题没有限制次数，从而$k = + \infty$，转移方程就简化为
```python
    dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
    dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
```

* 至多`k`次和交易满`k`次结果是一样的，因为如果不满`k`次，可以在同一天买入卖出无限次，收益保持不变。
