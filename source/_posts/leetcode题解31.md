---
title: leetcode题解31：下一个排列
date: 2022-05-08 12:11:57
tags:
    - leetcode
    - 数组
    - 二分查找
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第31题](https://leetcode-cn.com/problems/next-permutation/)

整数数组的一个排列就是将其所有成员以序列或线性顺序排列。

<!--more-->

例如，`arr = [1,2,3]` ，以下这些都可以视作 arr 的排列：`[1,2,3]、[1,3,2]、[3,1,2]、[2,3,1]` 。

整数数组的下一个排列是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的下一个排列就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。


例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。
给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**原地**修改，只允许使用额外常数空间。

 
示例 1：

> 输入：nums = [1,2,3]
输出：[1,3,2]

示例 2：

> 输入：nums = [3,2,1]
输出：[1,2,3]

示例 3：

> 输入：nums = [1,1,5]
输出：[1,5,1]


提示：

* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 100`

## 分析

仔细分析不难发现，有两个事实：
1. 所有排列中，最小的排列是升序排列，最大的排序是降序排列
2. 如果数组是降序排列，如`3 2 1`，则下一个更大的排列不存在，只能回到`1 2 3`。

从而若数组`a_1 a_2 ... a_{k-1} a_k .... a_n`中从`a_k`到`a_n`是降序，且`a_{k-1} < a_k`，从而`a_k ... a_n`已经是最大的排列了，那么按照字典排序，那么下一个排列应该是将`k-1`位置上的数换成比`a_{k-1}`大的数字，并且应该是`a_k .. a_n`中大于`a_{k-1}`的所有数中的最小值，不妨设`a_j`是该值，那么交换后的排列为`a_1 a_2 ... a_j a_k ... a_{k-1} ... a_n`，这时比较两个排列

```
a_1 a_2 ... a_{k-1} a_k ... a_j     ... a_n   （1）
a_1 a_2 ... a_j     a_k ... a_{k-1} ... a_n   （2）
```

经过前面的分析知道排列`(1)`的下一个排列必然是以`a_1 a_2 ... a_j`开始的，且后面序列`a_k ... a_{k-1} ... a_n`为最小排列即升序排列。又由于`a_k ... a_{k-1} ... a_n`是降序，从而将该序列逆序即可得到升序排列了。

比如求`3 5 4 2`的下一个排列，这时`5 4 2`是降序，而且`3 < 5`，由于`5 4 2`中比`3`大的数为`5 4`，其中最小值为`4`，所以`3`和`4`交换，排列变为`4 5 3 2`，再将`5 3 2`逆序，即得到下一个排列`4 2 3 5`。

所以算法主要分三步：
1. 从后往前找，找到第一个`a_i > a_{i-1}`，即第一个非降序的排列，记降序排列的起始位置为`desc_index`
2. 从后往前，找到在`desc_index`位置以后的数中第一个大于`desc_index-1`位置的数，并交换两个数
3. 逆序从`desc_index`开始的所有数

注意到第2步中，在降序排列中找数，可以使用二分查找，当然最简单的方法就是从后往遍历数组。


## 算法

<details open>
<summary>python3</summary>

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 从后往前找到第一个非降序排列的index
        desc_index = 0
        for i in range(len(nums)-1, 0, -1):
            if nums[i] > nums[i-1]:
                desc_index = i
                break

        # 使用二分查找，从后往前找到第一个比index位置大的数，并交换两个数
        if desc_index > 0:
            start = desc_index
            end = len(nums)
            while start < end:
                mid = (start + end) >> 1
                if nums[mid] > nums[desc_index-1]:
                    start = mid + 1
                elif nums[mid] <= nums[desc_index-1]:
                    end = mid

            find_index = start - 1
            nums[find_index], nums[desc_index-1] = nums[desc_index-1], nums[find_index]

        # 逆序从index开始的数组
        for i in range(0, (len(nums)-desc_index) // 2):
            nums[desc_index+i], nums[len(nums)-1-i] = nums[len(nums)-1-i], nums[desc_index+i]
```
</details>


<details>
<summary>c++</summary>

```c++
class Solution {
public:
	void nextPermutation(vector<int> &nums) {
		int i = nums.size() - 2;
		for (; i >= 0; i--) {
			if (nums[i+1] > nums[i])
				break;
		}
		if (i >=0) {
			int j;
			for (j = nums.size() - 1; j > i; j--) {
				if (nums[j] > nums[i])
					break;
			}
			int temp = nums[i];
			nums[i] = nums[j];
			nums[j] = temp;
		}
		for (int x = i + 1; x < (nums.size() + i + 1) / 2; x++) {
			int temp = nums[x];
			nums[x] = nums[nums.size() + i - x];
			nums[nums.size() + i - x] = temp;
		}
		return;
	}
};
```
</details>