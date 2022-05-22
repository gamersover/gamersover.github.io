---
title: leetcode题解41：缺失的第一个正数
date: 2022-05-21 11:29:20
tags:
    - leetcode
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第41题](https://leetcode.cn/problems/first-missing-positive/)
<!--more-->


## 分析

没有出现的最小的正整数，按照暴力法就是先判断`1`有没有在，没有则返回`1`，有则继续判断`2`，依次下去；但是有更好的方法，假设没有出现的最小正整数为`n`，表示`1～n-1`都在数组中，那么如果将`k`放到数组的`k-1`位置上，即`arr[k-1]=k`，其中`arr`为数组，数组的前`n-1`个数就是`1,2,...,n-1`，由于`n`不在数组中，那么`arr[n-1]`必不等于`n`。那么求解过程应该是
1. 遍历到`i`处，如果`1<=arr[i]<=len(arr)`，表示`arr[i]`应该放到`arr[i]-1`这个位置上，但是首先判断`arr[i]-1`是否已经是`arr[i]`，如果是的话不进行操作，继续遍历下一个；如果不是则交换`i`与`arr[i]-1`位置上的两个数，继续遍历下一个；
2. 当遍历完整个数组后，第一个不满足`arr[k-1]==k`的`k`就是解了；如果都满足表示`1~len(arr)`都在数组中，那么解就是`len(arr)+1`。


## 算法

<details open>
<summary>python</summary>

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        i = len(nums) - 1
        while i >= 0:
            if 0 <= nums[i] - 1 < len(nums) and nums[nums[i]-1] != nums[i]:
                j = nums[i] - 1
                nums[i], nums[j] = nums[j], nums[i]
            else:
                i -= 1
        for i in range(len(nums)):
            if nums[i] != i + 1:
                return i + 1
        return len(nums) + 1
```
</details>
