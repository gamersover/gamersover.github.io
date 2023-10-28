---
title: leetcode题解140：单词拆分II
date: 2023-10-01 11:36:42
tags:
    - 递归
    - 字符串
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第140题](https://leetcode.cn/problems/word-break-ii/)

<!--more-->

## 分析

这一题就是要求所有解的，所以我们使用递归。

假设要计算从`start`开始，到`end`结束的子串`s[start:end+1]`的所有解（大问题），先判断是否有前缀`s[start:start+n]`在`wordDict`中，如果在就继续递归剩下的的子串（小问题），并且返回子串分割的所有解，然后将前缀加入到每个解中，就是最终的解。

递归的解法还是大问题化解成小问题的解法，判断前缀的方法有两种：
1. 遍历n，然后判断是否在`wordDict`中；
2. 遍历`wordDict`，判断是否是某个前缀。

两种写法都可以，看题目的`wordDict`比`len(s)`大很多，所以第一种方法可能更好点。

然后就是递归的写法了，可以先返回子串分割的所有解，然后将前缀加入到每个解中；也可以在递归的过程中将前缀结果加到最终结果中，怎么写都可以，看习惯就行。

当然，在写的过程中会发现有些地方子串会重复计算，其中可以缓存下计算结果，也就是记忆化搜索，这里就不展开了。具体可以看官方题解。


## 代码

```python
class Solution:
    def wordBreak(self, s, wordDict):
        self.ans = []
        self.search2(s, wordDict, 0, [])
        return self.ans

    def search2(self, s, wordDict, start, res):
        if len(s) == start:
            self.ans.append(" ".join(res))
            return
        for i in range(start+1, len(s)+1):
            if s[start:i] in wordDict:
                self.search2(s, wordDict, i, res+[s[start:i]])
```