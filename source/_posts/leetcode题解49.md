---
title: leetcode题解49
date: 2022-07-23 15:22:46
tags:
    - leetcode
    - 字符串
    - 哈希表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第49题](https://leetcode.cn/problems/group-anagrams/)
<!--more-->

## 分析

该题思路很简单，就是如何判断两个字符串是字母异位词，就是两个单词含有的字符串是一样的，有很多判断方法，比如两个字符串排序后相等，又比如两个字符串含有的不同字母的个数是一样的，等等；将计算的值作为字典的key，然后key相等的就放到一起。


## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        d = {}
        for s in strs:
            k = "".join(sorted(s))
            d[k] = d.get(k, []) + [s]
        return list(d.values())
```