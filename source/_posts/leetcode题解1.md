---
title: leetcode题解1：两数之和
date: 2020-11-07 09:09:23
tags:
    - leetcode
    - 哈希表
    - 数组
categories: 算法
mathjax: true
---

## 题目描述

该题来自于[力扣第一题](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

<!--more-->

示例:
> 给定 nums = [2, 7, 11, 15], target = 9
> 
> 因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]


## 分析

如果原数组已经排好了序，这时可以使用左右双指针法，空间复杂度为$O(1)$，时间复杂度为$O(n)$；

但这里原数组无序，要想依然达到$O(n)$的时间复杂度，关键在于如何使用$O(1)$的时间复杂度在数组中查找元素并获取下标，这时自然会想到使用hash存储。

所以基本思路为：将原始数组存储为HashMap，之后只需遍历数组元素并查询数组中是否存在(**目标值**$-$**元素值**)的元素，如果存在取出相应下标即可。此时空间复杂度为$O(n)$，时间复杂度为$O(n)$。
    
## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def twoSum(self, nums, target):
        item_idx_map = {}
        for i in range(len(nums)):
            # 获取另一个数的下标，如果不存在返回None
            j = item_idx_map.get(target - nums[i], None)
            if j is not None:
                return [i, j]
            else:
                item_idx_map[nums[i]] = i
        return []
```
</details>


<details>
<summary>c++</summary>

```cpp
#include<vector>
#include<unordered_map>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++) {
            auto j = map.find(target - nums[i]);
            if (j != map.end()) {
                return {j->second, i};
            }
            else
                map[nums[i]] = i;
        }
        return {};
    }
};
```
</details>


<details>
<summary>java</summary>

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i = 0; i < nums.length; i++){
            if(map.containsKey(target - nums[i])){
                return new int[]{map.get(target - nums[i]), i};
            }
            else {
                map.put(nums[i], i);
            }
        }
        return new int[]{};
    }
}
```
</details>