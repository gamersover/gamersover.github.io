---
title: leetcode题解16：最接近的三数之和
date: 2021-03-13 10:42:01
tags:
    - leetcode
    - 数组
    - 双指针法
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第16题](https://leetcode-cn.com/problems/3sum-closest/)
给定一个包括`n`个整数的数组`nums`和一个目标值`target`。找出`nums`中的三个整数，使得它们的和与`target`最接近。返回这三个数的和。假定每组输入只存在唯一答案。

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

首先想到三数之和的解法，将数组排序，然后先固定第一个数，接下来就是如何找到另外两个数，使得三个数的和最接近target。还是用双指针法，假设第一个数下标为`first`，第二个数下标为`second=first+1`，第三个数下标为`third=n-1`，记`s=nums[first]+nums[second]+nums[third]`，由于数组是排过序的，所以分三种情况
1. 如果`s=target`，直接返回这`target`就可以了；
2. 如果`s>target`，那么移动指针`second`，只会让`s`更大，从而与`target`相差更大，所以让`third--`；
3. 如果`s<target`，那么移动指针`third`，只会让`s`更小，从而与`target`相差更大，所以让`second--`

经过上述步骤，如果没有返回，记录`s-target`的绝对值是否是最小的，并更新s，然后遍历`first`就可以找到最终结果了。
## 算法
1. 排序数组
2. 将最接近`target`的值`closest`初始化为`nums[0]+nums[1]+nums[2]`
3. 第一个数下标`first`从`0`到`n-3`遍历数组
4. 进入循环体，设`second=first + 1`，`third=n-1`，计算`s=nums[first] + nums[second] + nums[third]`
5. 判断`s`与`target`的关系，如果`s==target`，直接返回target，退出程序；否则按照上面的分析更新`second`，`third`
6. 如果`|s-target|<|closest-target|`，那么更新`closest=s`
7. 判断`second`是否大于等于`third`，如果是，退出循环；否则`first++`，并进入循环体（第4步）

小优化：
> 在三数之和中，为了避免找到重复解，每到一个新数时可以和前面数比较，如果一样直接跳过。这里在遍历`first,second,third`三个数时都可以跳过重复值，避免重复计算。
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