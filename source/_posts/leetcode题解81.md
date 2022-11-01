---
title: leetcode题解81：搜索旋转排序数组II
date: 2022-10-24 11:22:10
tags:
    - leetcode
    - 数组
    - 二分法
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第81题](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)

<!--more-->

## 分析

与[力扣33题题解](https://caoqinping.com/2022/05/14/leetcode%E9%A2%98%E8%A7%A333/)几乎一模一样，不同点在于数组中的元素可能相同，其实遇到相同的值时，不需要二分，直接遍历就好了；具体如下：
1. 初始化左右端点`left=0, right=len(nums)`，只要`left < right`进入循环，否则退出
2. 循环体：取`mid=(left+right)/2`，分三种情况
3. 当`nums[mid]==nums[target]`时，直接返回
4. 当`nums[mid]>nums[target]`时，
    4.1 如果中间比右边小，表示后一部分一定递增，那么`nums[target]`肯定只能在左边了，所以更新右边`right=mid`，继续二分左边
    4.2 中间大于右边，表示是旋转点，即前后有两个递增段，而`nums[target]`是比`nums[mid]`小的，那么如果`nums[right-1] < target`，表明右边的递增段已经不可能含有`nums[target]`，所以`right=mid`继续二分左边；如果`nums[right-1]>=target`，那么可能是在右边，从而`left = mid + 1`
    4.3 中间等于右边，这时候中间到右边的情况就不可知了，只知道右边不等于`nums[target]`，所以直接`right-=1`就好
5. 当`nums[mid]<nums[target]`时，类似第4点的分析就好，关键点在于判断确定的递增段在哪？

## 代码

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums)
        while left < right:
            mid = left + ((right - left) >> 1)
            if nums[mid] == target:
                return True
            elif nums[mid] > target:
                if nums[mid] < nums[right-1]:
                    right = mid
                elif nums[mid] > nums[right-1]:
                    if nums[right-1] < target:
                        right = mid
                    else:
                        left = mid + 1
                else:
                    right -= 1
            else:
                if nums[mid] > nums[left]:
                    left = mid + 1
                elif nums[mid] < nums[left]:
                    if nums[left] > target:
                        left = mid + 1
                    else:
                        right = mid
                else:
                    left += 1
        return False
```