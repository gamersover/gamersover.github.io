---
title: leetcode题解123：买卖股票的最佳时机 III
date: 2022-12-04 12:16:01
tags:
    - leetcode
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第123题](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

<!--more-->

## 分析

[力扣第122题题解](https://caoqinping.com/2022/12/04/leetcode题解122/)附录的通用解法的状态转移方程为：
```python
    dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
    dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])  # 由于买入算交易依次，从而是k-1次不持有的状态
```

该题中`k=2`，从而转移方程直接写出来，注意`dp[i-1][0][0]=0`因为不交易不持有当然是`0`
```python
    dp[i][2][0] = max(dp[i-1][2][0], dp[i-1][2][1] + prices[i])
    dp[i][2][1] = max(dp[i-1][2][1], dp[i-1][1][0] - prices[i])
    dp[i][1][0] = max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
    dp[i][1][1] = max(dp[i-1][1][1], -prices[i])
```

状态只依赖前一次，所以空间可以优化，注意更新顺序（就按照上面的顺序从上万下更新，正好不会影响状态之间的依赖）
```python
    dp[2][0] = max(dp[2][0], dp[2][1] + prices[i])
    dp[2][1] = max(dp[2][1], dp[1][0] - prices[i])
    dp[1][0] = max(dp[1][0], dp[1][1] + prices[i])
    dp[1][1] = max(dp[1][1], -prices[i])
```

甚至可以直接用四个变量来表示上面四个值，最终结果显示是`dp[2][0]`，交易了`2`次不持有股票的状态。

## 代码

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        sell1, sell2 = 0, 0
        buy1, buy2 = -prices[0], -prices[0]
        for i in range(1, len(prices)):
            sell2 = max(sell2, buy2 + prices[i])
            buy2 = max(buy2, sell1 - prices[i])
            sell1 = max(sell1, buy1 + prices[i])
            buy1 = max(buy1, -prices[i])
        return sell2
```