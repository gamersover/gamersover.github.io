---
title: leetcode题解16：最接近的三数之和
date: 2021-03-13 10:42:01
tags:
    - leetcode
    - 数组
    - 双指针
categories: 算法
mathjax: true
---

该题来自于[力扣第16题](https://leetcode-cn.com/problems/3sum-closest/)
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

<!--more-->
 

示例：
> 输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
 

提示：
* `3 <= nums.length <= 10^3`
* `-10^3 <= nums[i] <= 10^3`
* `-10^4 <= target <= 10^4`

## 分析

## 具体算法

## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def threeSumClosest(self, nums, target):
        nums.sort()
        closest = sum(nums[:3])
        first = 0
        while first < len(nums) - 2:
            second, third = first + 1, len(nums) - 1
            while second < third:
                s = nums[first] + nums[second] + nums[third]
                if abs(s - target) <= abs(closest - target):
                    closest = s
                    if closest == target:
                        return target
                if s > target:
                    while third > second and nums[third] == nums[third-1]:
                        third -= 1
                    third -= 1
                elif s < target:
                    while second < third and nums[second] == nums[second+1]:
                        second += 1
                    second += 1
            
            while first < len(nums) - 2 and nums[first] == nums[first+1]:
                first += 1
            first += 1
        
        return closest
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int first = 0;
        int closest = nums[0] + nums[1] + nums[2];
        while (first < nums.size() - 2) {
            int second = first + 1;
            int third = nums.size() - 1;
            while (second < third) {
                int s = nums[first] + nums[second] + nums[third];
                if (abs(s - target) <= abs(closest - target)) {
                    closest = s;
                    if (closest == target) return target;
                }
                if (s > target) {
                    while((third > second) && (nums[third] == nums[third-1])) third--;
                    third--;
                }
                else if(s < target) {
                    while ((second < third) && (nums[second] == nums[second + 1])) second++;;
                    second++;
                }
            }
            while ((first < nums.size() - 2) && (nums[first + 1] == nums[first])) first++;
            first++;
        }
        return closest;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int first = 0;
        int closest = nums[0] + nums[1] + nums[2];
        while (first < nums.length - 2){
            int second = first + 1;
            int third = nums.length - 1;
            while (second < third){
                int s = nums[first] + nums[second] + nums[third];
                if (Math.abs(s - target) <= Math.abs(closest - target)){
                    closest = s;
                }
                if (closest == target) return target;
                if (s > target){
                    while ((third > second) && (nums[third] == nums[third-1])) third--;
                    third--;
                }
                else if (s < target){
                    while ((second < third) && (nums[second] == nums[second+1])) second++;
                    second++;
                }
            }
            while ((first < nums.length - 2) && (nums[first] == nums[first+1])) first++;
            first++;
        }
        return closest;
    }
}
```
</details>