---
title: leetcode题解93：复原IP地址
date: 2022-11-05 11:22:27
tags:
    - leetcode
    - 字符串
    - 深度优先遍历
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第93题](https://leetcode.cn/problems/restore-ip-addresses/)

<!--more-->

## 分析

很明显可以看出又是一个大问题逐渐化解成相似的小问题的解法，要找出`3`个切割点，可以先找出第一个切割点，然后剩余的字符串中找出其他`2`个切割点，所以该题解法本质是深度优先遍历的递归解法。

具体方法：
1. 找出所有符合条件的第一个切割点；
2. 遍历所有切割点，获取剩下的字符串，继续1直到字符串为空，表示切割完成；如果字符串不为空，表示切割失败。

理解了上述过程，其余部分就简单了，具体看代码。


## 代码

```python
class Solution:
    def find_dots(self, s):
        dots = []
        for i in range(1, len(s)+1):
            if i == 1:
                dots.append(i)
            if i == 2 and s[0] != '0':
                dots.append(i)
            if i == 3 and ("100" <= s[:3] <= "255"):
                dots.append(i)
        return dots

    def restore(self, s, n):
        ans = []
        if n == 0:
            if len(s) == 0:
                ans.append("")
            return ans
        dots = self.find_dots(s)
        for dot in dots:
            subans = self.restore(s[dot:], n-1)
            for sub in subans:
                if sub == "":
                    ans.append(s[:dot])
                else:
                    ans.append(s[:dot] + "." + sub)
        return ans

    def restoreIpAddresses(self, s: str) -> List[str]:
        return self.restore(s, 4)
```