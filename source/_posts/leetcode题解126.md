---
title: leetcode题解126：单词接龙 II
date: 2023-02-04 10:57:53
tags:
    - leetcode
    - 队列
    - 广度优先搜索
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第126题](https://leetcode.cn/problems/word-ladder-ii/)

<!--more-->

## 分析

仔细思考会发现是一个图问题，单个字母不同的意味着之间有联系，而该题要求接最短路径，给出了起始节点和结束节点，那么首先想到的是广度优先搜索BFS。

首先看看BFS的过程，BFS可以用队列来模拟，比如从起始节点`a`出发，
1. 第一层（即最短路径为1的节点）`b, c`
2. 那么第二层就是遍历第一层的所有节点，并去掉访问过的节点（即路径为2的节点），`e, f, g, h`

其中`b`的相邻节点为`e,f`，`c`的相邻节点为`g,h`，注意需要去掉已访问的节点。依次迭代下去，可以求出从起始节点到任意节点的最短路径，这就是BFS的特点。

那么该题用BFS模拟的话，开始路径初始为`paths =[ [beginWord] ]`，只有一个节点，
1. 取beginWord的所有相邻节点，添加到路径，假设结果为`paths=[ [beginWord, node1], [beginWord, node2] ]`
2. 记录访问过的节点`node1, node2`，
3. 在遍历下一层之间，注意从所有节点中去掉访问过的节点，因为假设到达之前层节点的路径为`k`，那么如果从下一层回到之间层节点那么路径为`k+1`，显然路径变大了，就不是最小路径了，所以没必要继续保留访问过的节点
4. 从上一次的结果`paths`遍历，假设新结果为`[ [beginWord, node1, node3],  [beginWord, node2, node3], [beginWord, node2, node4] ]`，过程就是遍历每个路径，然后取路径的最后一个节点，然后找该节点的相邻结点，这里`node1, node3`是相邻结点，`node2`分别和`node3, node4`为相邻结点
5. 一直迭代下去，直到遇到结束节点退出。

这里还有两个重要细节和优化点：
1. 对于`paths`完全可以用队列模拟，先进的出队列，对出去的元素求下一层的路径，并添加入队列（添加到队列尾部）
2. 对于第一点唯一的问题在于用单个队列模拟后，怎么确定当前元素在哪一层，从而好去掉访问过的节点？其实也很简单，元素数组的长度就是当前层的路径长度，只有达到下一层的时候，路径长度才会变化，所以只需要判断当前元素数组的长度是否大于之前的长度即可
3. 怎么高效从一堆节点中找到与当前节点相邻的节点呢？如果两两对比，复杂度太高；优化点就是对当前单词的每个字母变换成`a-z`中的一个，然后判断`wordList`中是否存在变换后的节点即可。

## 代码

```python
class Solution:
    def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
        paths = [[beginWord]]  # BFS的结果
        level = 1 # BFS层数
        wordlist = set(wordList)  # BFS下一层可以使用的wordlist
        words = set() # BFS下一层使用过的words
        min_level = float("inf")  # 最小路径
        ans = [] # 最终结果
        alphabeta = [chr(i) for i in range(ord('a'), ord('a')+26)]
        while len(paths)>0:
            path = paths.pop(0)
            if len(path) > level: # 开始处理BFS下一层
                # 从wordlist中删除已经使用过的words
                for w in words:
                    wordlist.discard(w)
                words.clear()
                level = len(path) # 更新当前处理层数
                if level > min_level: # BFS层数已经超过最小路径，直接结束程序
                    break

            # 开始寻找path最后一个元素的相邻元素
            last = path[-1]
            for i in range(len(last)):
                for c in alphabeta:
                    new_last = last[:i] + c + last[i+1:]
                    if new_last not in wordlist:
                        continue
                    words.add(new_last)
                    new_path = path + [new_last]
                    if new_last == endWord:
                        ans.append(new_path)
                        min_level = level
                    else:
                        paths.append(new_path)
        return ans
```
