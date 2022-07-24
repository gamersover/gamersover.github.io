---
title: leetcode题解51
date: 2022-07-24 12:27:00
tags:
    - leetcode
    - 递归
    - 回溯
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第51题](https://leetcode.cn/problems/n-queens/)
<!--more-->

## 分析

首先每个皇后只能在一行，同一行的不行，那么就可以用一个长度为`n`的数组记录当前的状态，比如`[1, 3, 0, 2]`表示，第`0`、`1`、`2`，`3`个皇后分别在`1`、`3`、`0`、`2`列；要放置`n`个皇后，大致用一下过程解决：
1. 由于每行必有且只有一个皇后，所以在第一行放置一个，可以放置到任意列；
2. 考虑下一行的皇后能够放到的位置，即可以放置的列有哪些？遍历所有可能，并继续考虑下一行

这样的过程，可以考虑为一棵树，然后深度优先遍历这棵树，得到的就是所有可能的方案；考虑遍历到其中某个结点`node`时，当前`k`个皇后的放置状态为数组`arr`，那么先找出第`k+1`个皇后的所有可能列`candidates`，然后遍历`candidates`，首先将第`k+1`个皇后的状态更新到`arr`，然后判断`arr`的长度是否达到了`n`，如果是，表明找到了一个解，记录下来；没有继续放置`k+2`，当`node`结点下面的所有可能都遍历完后，表示`node`结点已经完成了，应该要更新`node`结点的兄弟结点了，这是需要还原第`k`个皇后放置前的状态，即回溯下`arr`。

在已知前`k`个皇后状态数组`arr`的情况下，如何找到第`k+1`个皇后的所有可能列呢？仔细推导下发现，同一列的去掉，同一斜线的去掉，同一列的简单，就是`arr`中的值，同一斜线的就是列之差的绝对值和行之差的绝对值相等。


## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        ans = []
        self.dfs([], n, ans)
        return ans

    def dfs(self, all_cols, n, ans):
        for c in self.find_candidates(all_cols, n):
            all_cols.append(c)
            if len(all_cols) == n:
                ans.append(self.format_ans(all_cols))

            self.dfs(all_cols, n, ans)
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

    def format_ans(self, ans):
        s = ['.'*i + 'Q' + '.'*(len(ans)-i-1) for i in ans]
        return s
```