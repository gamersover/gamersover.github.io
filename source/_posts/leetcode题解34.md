---
title: leetcode题解34：在排序数组中查找元素的第一个和最后一个位置
date: 2022-05-14 15:55:24
tags:
    - leetcode
    - 数组
    - 二分法
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第34题](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

<!--more-->

## 分析
典型的二分查找，要找第一个，那就当`nums[mid]>=target`，选左边，否则选右边；要找最后一个，那就当`nums[mid] <= target`时，选右边，否则选左边。

## 算法

<details open>
<summary>python</summary>

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def find_first(nums, target):
            left, right = 0, len(nums)
            while left < right:
                mid = (left + right) >> 1
                if nums[mid] >= target:
                    right = mid
                else:
                    left = mid + 1

            if left < len(nums) and nums[left] == target:
                return left
            return -1

        def find_last(nums, target):
            left, right = 0, len(nums)
            while left < right:
                mid = (left + right) >> 1
                if nums[mid] <= target:
                    left = mid +1
                else:
                    right = mid

            if 0 <= left - 1 < len(nums) and nums[left-1] == target:
                return left - 1
            return -1

        arr = [find_first(nums, target), find_last(nums, target)]
        return arr

```
</details>

