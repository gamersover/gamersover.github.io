---
title: leetcode题解80：删除有序数组中的重复项II
date: 2022-10-24 10:54:23
tags:
    - leetcode
    - 双指针法
    - 数组
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第80题](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

<!--more-->

## 分析

根据题意，需要使用`O(1)`的空间复杂度且在原数组上修改；比如原数组`[1,1,1,2,2,3]`，那么修改后的数组应该是`[1,1,2,2,3]`，返回数组的长度`5`，超过该长度的元素不需要考虑。

考虑到复杂度，只能将后面的元素左移，一旦有出现一个超过`2`次的元素，那么该元素后面的元素都必须左移一位，也即后面的元素必须放到前面的某个位置上；这里使用双指针法来实现，慢指针`slow`指针前面需更换的位置，快指针`fast`指向需要移动到前面的元素位置，当满足一定条件时，就使得`nums[slow]=nums[fast]`即可；

问题是条件是什么？只需看如果将`fast`指向的元素移动到`slow`位置后，`slow`位置只需的元素会不会超过两个，所以只能是`nums[fast]!=nums[slow-2]`，此时移动过去之后，`slow`位置只需的元素个数至多是`2`个。

## 代码

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) < 3:
            return len(nums)
        slow, fast = 2, 2
        for fast in range(2, len(nums)):
            if nums[fast] != nums[slow-2]:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```


