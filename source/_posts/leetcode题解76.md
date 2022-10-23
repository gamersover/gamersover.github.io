---
title: leetcode题解76：最小覆盖子串
date: 2022-09-28 19:52:00
tags:
    - leetcode
    - 字符串
    - 滑动窗口
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第76题](https://leetcode.cn/problems/minimum-window-substring/)

<!--more-->

## 分析

该题简单在于能立刻想到应该使用滑动窗口解决，难点在于如何确定窗口刚好涵盖字符串`t`；当然可以统计窗口中元素的个数与`t`中的元素个数是否完全一致，但是这样每次对比都要遍历`t`中所有不同元素，有没有更高效的方法呢？

为了描述方便，记`cnt_map`是`t`中字符与其个数的字典，`window_cnt_map`是当前窗口中字符与其个数的字典，现看看窗口的左右端点是什么情况？

* 左端点：既然是求覆盖`t`的最小子串，那显然左端点的字符`leftChar`总是在`t`中，且左端点的字符个数`window_cnt_map[leftChar]`必小于等于`cnt_map[leftChar]`；

* 右端点：右端点会遍历整个字符串`s`，遍历时需要记录当前窗口每个字符的个数即`window_cnt_map`，同时如果右端点字符`rightChar`在`t`中，且`window_cnt_map[rightChar]<=cnt_map[rightChar]`，记`cnt`为有效的字符个数，表示窗口中涵盖`t`中字符的个数，显然此时`cnt`应该`+1`，当`cnt`等于`t`的长度时就找到了一个涵盖`t`的子串了。

这里`cnt`只需要`+1`操作的原因在于，左端点总是不包含多余的字符，保证了`window_cnt_map`中的每个字符的个数不会减少，即`cnt`不会减少，只会保持不变或者`+1`，而需不需要`+1`就是由右端点决定的。

遍历过程中可能会找到多个涵盖`t`的子串，只要保留最小的那个就是结果。

## 代码

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        cnt_map = dict()
        for c in t:
            cnt_map[c] = cnt_map.get(c, 0) + 1

        substr = ""
        min_len = len(s) + 1
        cnt, j = 0, 0
        window_cnt_map = dict()
        for i, ch in enumerate(s):
            window_cnt_map[ch] = window_cnt_map.get(ch, 0) + 1

            if ch in cnt_map and window_cnt_map[ch] <= cnt_map[ch]:
                cnt += 1

            while j <= i and (s[j] not in cnt_map or window_cnt_map[s[j]] > cnt_map[s[j]]):
                window_cnt_map[s[j]] -= 1
                j += 1

            if cnt == len(t):
                if i - j + 1 < min_len:
                    min_len = i - j + 1
                    substr = s[j:i+1]
        return substr
```

