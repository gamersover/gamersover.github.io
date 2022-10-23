---
title: leetcode题解79：单词搜索
date: 2022-10-23 12:23:23
tags:
    - leetcode
    - 递归
    - 回溯
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第79题](https://leetcode.cn/problems/word-search/)

<!--more-->

## 分析

典型的深度优先遍历（dfs），从当前位置出发，找到所有有效位置，然后继续深度优先遍历所有有效位置，直到找到匹配的。

方法如下：
1. 遍历网格，找到等于`word[0]`的位置，记为`i,j`，从该位置出发，开始`dfs`，
2. `dfs`的内容就是找出所有有效的位置，有效位置就是指：与当前位置相邻的位置且字符等于`word`中下一个字符的元素。

下一个位置可能会有多个，记作数组`next_pos`，由于到下一个位置的时候，之前的位置`i,j`也属于相邻位置，所以为了避免回到之前的位置，需要记录该位置是否已经访问过了，而访问过的位置当然就是无效的，`dfs`时无视；

所以方法改进如下：
1. 用一个二维数组记录当前位置是否访问过，还需记录`dfs`路径的长度
2. 遍历，找到等于`word[0]`的位置，并将该位置设置为已访问，从该位置出发，开始`dfs`，
3. 找到所有的下个有效位置记为数组`next_pos`，遍历`next_pos`，下个位置记为`pos`
4. 将`pos`设置为已访问，从`pos`开始继续`dfs`，直到路径的长度等于`word`的长度退出；

上述方法还有个问题，模拟`dfs`时，当从`next_pos[0]`位置开始`dfs`，当这条路径不正确时会退回到`next_pos[1]`位置，而`next_pos[0]`在之前已经设置为已访问，但是当退回到`next_pos[1]`时，`next_pos[0]`不应该时访问过的状态，因为先到`next_pos[1]`再到`next_pos[0]`可能是一条正确的路径，所以此时需要回溯，所以上述方法加上第5条；

5. 当`dfs`没有找到时，`pos`设置为未访问，继续遍历`next_pos`下一个元素，回到4

## 代码

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ncols, nrows = len(board), len(board[0])
        visited = [[False for _ in range(nrows)] for _ in range(ncols)]
        for i in range(ncols):
            for j in range(nrows):
                if board[i][j] == word[0]:
                    visited[i][j] = True
                    if self.dfs(board, i, j, 1, word, visited):
                        return True
                    visited[i][j] = False
        return False


    def dfs(self, board, i, j, n, word, visited):
        deltas = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        if n == len(word):
            return True

        next_pos = []
        for delta in deltas:
            next_i = i + delta[0]
            next_j = j + delta[1]
            if 0 <= next_i < len(board) and 0 <= next_j < len(board[0]) \
                and not visited[next_i][next_j] and board[next_i][next_j] == word[n]:
                next_pos.append((next_i, next_j))

        for pos in next_pos:
            visited[pos[0]][pos[1]] = True
            if self.dfs(board, pos[0], pos[1], n+1, word, visited):
                return True
            visited[pos[0]][pos[1]] = False
        return False
```


