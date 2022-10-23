---
title: leetcode题解75：颜色分类
date: 2022-09-28 19:33:14
tags:
    - leetcode
    - 数组
    - 双指针法
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第75题](https://leetcode.cn/problems/sort-colors/)

<!--more-->

## 分析

`O(n)`的时间复杂度解法，需要一定技巧，具体就是使用双指针法（实际上应该是三指针），一个指针指向头部，用于表示已放置好的`0`的位置；另一个指针指向尾部，用于表示已放置好的`2`的位置，这样再使用一个指针遍原数组，有以下三种情况：
1. 如果当前指针指向的值为`0`，此时将当前指针指向的值与头部指针指向的值交换，并且头部指针右移一位，表示一个`0`放好了，此时还需要将当前指针也右移一位；
2. 同样地，如果当前指针指向的值为`2`，则和尾部指针交换，并且尾部指针左移一位，表示一个`2`放好了；
3. 否则，不进行交换，当前指针右移一位。这样一直下去直到当前指针触碰到尾部指针。

注意，当前指针右移只能在1,3两种情况下，第1种情况中，可以右移的原因在于要交换的头部指针指向的值不可能是`2`，只能是`0,1`，因为如果是`2`的话，早就在第3种情况下交换到后面去了；第2中情况中不能右移的原因在于有可能后面的`0`交换到当前位置了。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        right = len(nums) - 1
        left, curr = 0, 0
        while curr <= right:
            if nums[curr] == 0:
                nums[left], nums[curr] = nums[curr], nums[left]
                left += 1
                curr += 1
            elif nums[curr] == 2:
                nums[right], nums[curr] = nums[curr], nums[right]
                right -= 1
            else:
                curr += 1
```




