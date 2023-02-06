---
title: leetcode题解127：单词接龙
date: 2023-02-04 11:46:26
tags:
    - leetcode
    - 递归
    - 广度优先搜索
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第127题](https://leetcode.cn/problems/word-ladder/)

<!--more-->

## 分析

有了[第126题](https://caoqinping.com/2023/02/04/leetcode题解126/)的解法后，该题就简单多了，由于不需要记录具体路径，从而只需要记录最后一个节点即可。例子如下：

1. 初始为`paths = [beginWord]`
2. 取beginWord的所有相邻节点，添加到路径，假设结果为`paths=[node1, node2]`
3. 记录访问过的节点`node1, node2`，
4. 在遍历下一层之间，注意从所有节点中去掉访问过的节点，
5. 从上一次的结果`paths`遍历，得到新的相邻节点，假设新结果为`[node3, node4]`
6. 一直迭代下去，直到遇到结束节点退出。

这里的`paths`只记录的当前层的节点，所以当遍历完上一层的`paths`时，层数更新即可。


## 代码

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        alphabeta = [chr(i) for i in range(ord('a'), ord('a')+26)]
        word_list = set(wordList)
        level_arr = [beginWord]     # 当前层节点
        visited = set([beginWord])  # 访问过的结点
        res = 0
        while len(level_arr) > 0:
            res += 1
            level_len = len(level_arr)
            while level_len > 0:
                s = level_arr.pop(0)
                for i in range(len(s)):
                    for c in alphabeta:
                        new_s = s[:i] + c + s[i+1:]
                        if new_s in word_list and new_s == endWord:
                            return res + 1
                        if new_s not in word_list or new_s in visited:
                            continue
                        level_arr.append(new_s)
                        visited.add(new_s)
                level_len -= 1
        return 0
```

