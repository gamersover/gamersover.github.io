---
title: leetcode题解52
date: 2022-07-24 12:49:04
tags:
    - leetcode
    - 递归
    - 回溯
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第52题](https://leetcode.cn/problems/n-queens-ii/)
<!--more-->

## 分析

思路和[第51题](https://caoqinping.com/2022/07/24/leetcode%E9%A2%98%E8%A7%A351/)一样，只不过不是输出数组，而是计数

## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        self.ans = 0
        self.dfs([], n)
        return self.ans

    def dfs(self, all_cols, n):
        for c in self.find_candidates(all_cols, n):
            all_cols.append(c)
            if len(all_cols) == n:
                self.ans += 1

            self.dfs(all_cols, n)
            all_cols.pop()

    def find_candidates(self, all_cols, n):
        all_valid = []
        for i in range(n):
            if self.isvalid(all_cols, i):
                all_valid.append(i)
        return all_valid

    def isvalid(self, all_cols, col):
        row = len(all_cols)
        for r, c in enumerate(all_cols):
            if c == col or abs(row - r) == abs(col - c):
                return False
        return True
```