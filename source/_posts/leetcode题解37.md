---
title: leetcode题解37：解数独
date: 2022-05-14 16:01:41
tags:
    - leetcode
    - 数组
    - 深度优先遍历
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第37题](https://leetcode.cn/problems/sudoku-solver/)
<!--more-->

## 分析
典型的深度优先遍历，假设递归函数为`solve`，那么递归的方式为：
1. 进入递归`solve`
2. 遍历`board`，找到待补充的区域，记作`(i, j)`，
3. 找到`(i,j)`的所有可选数字，记作`candidate_nums`，遍历所有`candidate_nums`，若对当前遍历的数字`n`，将`board[i][j]=n`，然后判断`solve`函数返回值，若为`True`，即表示成功解决数独，函数返回`True`；
4. 若遍历完所有`candidate_nums`，结果还是没有返回`True`，则表示前面的数字有误，则回溯还原现场`board[i][j]="."`，并且函数返回`False`，这样会回到递归上一层
5. 最终由于函数遍历完所有的待补充区域了，所以直接返回`True`

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def find_possible_number(board, i, j):
            all_number = set(map(str, range(1, 10)))
            for index in range(9):
                if board[index][j] != ".":
                    all_number.discard(board[index][j])
                if board[i][index] != ".":
                    all_number.discard(board[i][index])
            row = i // 3
            col = j // 3
            for m in range(row*3, (row+1)*3):
                for n in range(col*3, (col+1)*3):
                    if board[m][n] != ".":
                        all_number.discard(board[m][n])
            return all_number

        def solve(board):
            for i in range(9):
                for j in range(9):
                    if board[i][j] == ".":
                        for n in find_possible_number(board, i, j):
                            board[i][j] = n
                            if solve(board):
                                return True
                        board[i][j] = "."
                        return False
            return True

        solve = solve(board)

```
</details>
