---
title: leetcode题解55：跳跃游戏
date: 2022-08-02 19:06:48
tags:
    - leetcode
    - 数组
    - 贪心
---

## 描述

该题来自于[力扣第55题](https://leetcode-cn.com/problems/jump-game/)
<!--more-->

## 分析

比起[力扣第45题](https://leetcode-cn.com/problems/jump-game-ii/)，这道题更简单，只需要判断能否到达最后一个位置即可。每次只需记录能到达的最远位置即可。如果当前位置超过了最远位置，说明无法到达当前位置，也就无法到达最后一个位置；如果最远位置超过了最后一个位置，说明可以到达最后一个位置。

## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        max_, i = 0, 0
        max_ = i + nums[i]
        while (i + 1) <= max_ and max_ < len(nums) - 1:
            i += 1
            if i <= max_:
                max_ = max(max_, i + nums[i])
            else:
                return False
        return max_ >= len(nums) - 1
```