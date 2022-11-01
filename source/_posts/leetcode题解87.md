---
title: leetcode题解87：扰乱字符串
date: 2022-10-29 11:46:03
tags:
    - leetcode
    - 字符串
    - 递归
    - 深度优先遍历
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第87题](https://leetcode.cn/problems/scramble-string/)

<!--more-->

## 分析

没什么好的办法，直接递归暴力，遍历切割点，然后判断字符串的两边是否互为扰乱字符串就好。比如`great`和`rgeat`，如果切割点在`g reat`，那么递归查看`g`与`r`，`reat`与`geat`是否互为扰乱字符串？还有一种就是翻转了，即递归查看`g`与`t`，`reat`与`rgea`是否互为扰乱字符串？两种情况一种满足则返回`True`。都不满足则继续遍历下一个切割点`gr eat`。

注意到两个字符串互为扰乱串，长度比如相同，所以可以使用三个入参的函数`bool func(i1,i2,length)`表示字符串`s1[i1:i1+length]`与字符串`s2[i2:i2+length]`是否互为扰乱串，然后递归调用`func`就好。

该题如果直接递归的话会超时，需要一些小技巧，注意到在递归过程中，可能会涉及到重复计算，会重复判断两个字符串是否是扰乱字符串，所以可以将计算后的结果存储下来，最简单的方法使用一个多维数组记录递归的结果，对于`python`，可以使用`functools.cache`装饰器来缓存已经计算好的结果。

## 代码

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        @cache
        def dfs(i1, i2, length):
            if s1[i1:i1+length] == s2[i2:i2+length]:
                return True
            if sorted(s1[i1:i1+length]) != sorted(s2[i2:i2+length]):
                return False
            for i in range(1, length):
                if dfs(i1, i2, i) and dfs(i1+i, i2+i, length-i):
                    return True
                if dfs(i1, i2+length-i, i) and dfs(i1+i, i2, length-i):
                    return True
            return False

        ans = dfs(0, 0, len(s1))
        dfs.cache_clear()
        return ans
```