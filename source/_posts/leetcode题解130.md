---
title: leetcode题解130：被围绕的区域
date: 2023-02-05 12:07:58
tags:
    - leetcode
    - 二叉树
    - 递归
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第130题](https://leetcode.cn/problems/surrounded-regions/)

<!--more-->

## 分析

要求的是被`X`围绕的区域，边界的不算。正面求解不好求，那就反面求解，即求出所有边界的`O`。

思路就很明了了，找出边界的`O`，然后从这些`O`开始深度优先遍历，找到所有相邻的`O`，可以标记下找出的`O`，那么最后将这些没有标记的`O`转为`X`就可以了。

深度优先遍历就很简单了，找出所有相邻的`O`，继续递归遍历即可。

## 代码

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        m, n = len(board), len(board[0])
        def is_valid_pos(x, y, m, n):
            if 0 <= x < m and 0 <= y < n and board[x][y] == 'O':
                return True
            return False

        def find_valid_pos(x, y, m, n):
            all_pos = []
            if is_valid_pos(x-1, y, m, n):
                all_pos.append((x-1, y))
            if is_valid_pos(x+1, y, m, n):
                all_pos.append((x+1, y))
            if is_valid_pos(x, y-1, m, n):
                all_pos.append((x, y-1))
            if is_valid_pos(x, y+1, m, n):
                all_pos.append((x, y+1))
            return all_pos


        def dfs(board, x, y, m, n):
            board[x][y] = '|'
            for x, y in find_valid_pos(x, y, m, n):
                dfs(board, x, y, m, n)

        for i in range(m):
            if board[i][0] == 'O':
                dfs(board, i, 0, m, n)
        for i in range(m):
            if board[i][n-1] == 'O':
                dfs(board, i, n-1, m, n)
        for j in range(n):
            if board[0][j] == 'O':
                dfs(board, 0, j, m, n)
        for j in range(n):
            if board[m-1][j] == 'O':
                dfs(board, m-1, j, m, n)

        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == '|':
                    board[i][j] = 'O'
```