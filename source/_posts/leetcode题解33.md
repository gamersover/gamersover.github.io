---
title: leetcode题解33：搜索旋转排序数组
date: 2022-05-14 15:06:36
tags:
    - leetcode
    - 数组
    - 二分法
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第33题](https://leetcode.cn/problems/search-in-rotated-sorted-array)

整数数组 nums 按升序排列，数组中的值互不相同 。
<!--more-->

在传递给函数之前，nums 在预先未知的某个下标 `k(0 <= k < nums.length)`上进行了旋转，使数组变为`[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 从 0 开始 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 3 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你旋转后的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

 
示例 1：

> 输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4

示例 2：

>输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1

示例 3：

> 输入：nums = [1], target = 0
输出：-1


提示：

* 1 <= nums.length <= 5000
* -10^4 <= nums[i] <= 10^4
* nums 中的每个值都 独一无二
* 题目数据保证 nums 在预先未知的某个下标上进行了旋转
* -10^4 <= target <= 10^4
 

进阶：你可以设计一个时间复杂度为 O(log n) 的解决方案吗？


## 分析

类似于排序数组，依然可以使用二分查找法，只不过考虑的情况更多而已，在一个旋转排序数组中，我们考虑`left, mid, right`，还是两种情况：
1. 如果`nums[mid] > target`：
   - 如果`nums[left]>nums[mid]`，表示旋转点必在`left`和`mid`之间，所以`mid`后的数是递增的，从而`target`必在`mid`左边
   - 如果`nums[left]<nums[mid]`，表示`left`到`mid`是递增的，所以如果`nums[left] <= target`，即`nums[left] <= target < nums[mid]`，表明`target`必在`mid`左边
   - 如果`nums[left]<nums[mid]`且`nums[left] > target`，则`target`必不可能在`mid`左边，只能出现在右边了
2. 如果`nums[mid] < target`，可以仿照`1`的分析，看什么条件下，一定是递增的，从而判断`target`在`mid`的左边还是右边


## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)
        while left < right:
            mid = (left + right) >> 1
            if nums[mid] > target:
                if nums[mid] > nums[left]:
                    if nums[left] <= target:
                        right = mid
                    else:
                        left = mid +1
                else:
                    right = mid
            elif nums[mid] < target:
                if nums[mid] > nums[left]:
                    left = mid + 1
                else:
                    if nums[right-1] >= target:
                        left = mid + 1
                    else:
                        right = mid
            else:
                return mid
        return -1

```
</details>
