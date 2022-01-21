---
title: leetcode题解15：三数之和
date: 2021-01-22 20:16:33
tags:
    - leetcode
    - 数组
    - 双指针法
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第15题](https://leetcode-cn.com/problems/3sum)
给你一个包含`n`个整数的数组`nums`，判断`nums`中是否存在三个元素`a`，`b`，`c` ，使得`a + b + c = 0`？请你找出所有和为`0`且不重复的三元组。

注意：答案中不可以包含重复的三元组。
<!--more-->


示例 1：
> 输入：nums = [-1,0,1,2,-1,-4]
输出：\[[-1,-1,2],[-1,0,1]]

示例 2：
> 输入：nums = []
输出：[]

示例 3：
> 输入：nums = [0]
输出：[]
 

提示：

* `0 <= nums.length <= 3000`
* `-105 <= nums[i] <= 105`


## 分析
思考关键点在于确定了第一个数后，相当于找到和为固定的其他两个数，就可以联想到两数之和的做法了。回顾下两数之和的做法：先对数组排序，然后使用双指针法。由于该题需要找出所有满足条件的三元组，那么就需要找到所有满足条件的两数。假设初始状态下，左右指针分别指向`1,4`，如下：

> <font color='red'>1</font> 1 2 2 3 4 <font color='blue'>4</font>

这时使用双指针法有以下三种情况：
* 左指针left指向的数与右指针指向的数之和大于目标值4，这时需要将右指针向左移动
* 左指针left指向的数与右指针指向的数之和小于目标值6，这时需要将左指针向右移动
* 左指针left指向的数与右指针指向的数之和等于目标值5，这时记录这个两数。为了找到所有满足条件的两个数（而且不能重复），需要继续遍历，这时显然将左指针向右移动直到所指向的值不是原来的值，将右指针向左移动直到所指向的值不是原来的值，此时左右指针分别指向`2,3`
    > 1 1 <font color='red'>2</font> 2 <font color='blue'>3</font> 4 4

如此下去，直到左指针遇到右指针，就找到了所有满足条件的两个数了。

在遍历第一个数时，为了避免找到重复解，每到一个新数时可以和前面数比较，如果一样直接跳过，因为前面已经遍历过该数的情况了。

## 算法
1. 对数组排序
2. 遍历数组，下标`i`从`0`到`nums.length - 3`，如果`i>0 && nums[i]==nums[i-1]`，表示该数和前面一样，跳过该下标，遍历`i+1`
3. 进入循环，找到两数之和为`-nums[i]`，初始化左指针`left=i+1`，右指针`right=nums.length-1`，循环条件左指针比右指针小`left<right`
4. 如果`nums[left]+nums[right]>-nums[i]`，这时移动右指针`right--`
5. 如果`nums[left]+nums[right]<-nums[i]`，这时移动右指针`left++`
6. 如果`nums[left]+nums[right]==-nums[i]`，记录三元组`[[nums[i], nums[left], nums[right]]`，并移动左指针直到`nums[left]!=nums[left+1]`，移动右指针直到`nums[right]!=nums[right-1]`
7. 回到3判断循环是否结束，如果结束回到2，否则继续4


## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def threeSum(self, nums):
        nums.sort()
        res = []
        for i in range(0, len(nums)-2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            left = i + 1
            right = len(nums) - 1
            while left < right:
                if nums[left] + nums[right] > -nums[i]:
                    right -= 1
                elif nums[left] + nums[right] < -nums[i]:
                    left += 1
                else:
                    res.append([nums[i], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left+1]:
                        left += 1
                    while left < right and nums[right] == nums[right-1]:
                        right -= 1
                    left += 1
                    right -= 1
        return res
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int> &nums) {
        if (nums.size() < 3) return {};
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        for (int i = 0; i < nums.size() - 2; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                if (nums[left] + nums[right] > -nums[i]) right--;
                else if (nums[left] + nums[right] < -nums[i]) left++;
                else {
                    res.push_back({nums[i], nums[left], nums[right]});
                    while ((left < right) && (nums[left] == nums[left+1])) left++;
                    while ((left < right) && (nums[right] == nums[right-1])) right--;
                    left++; right--;
                }
            }
        }
        return res;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums.length < 3)
            return res;
        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                if (nums[left] + nums[right] > -nums[i])
                    right--;
                else if (nums[left] + nums[right] < -nums[i])
                    left++;
                else {
                    List<Integer> tmp = new ArrayList<>();
                    tmp.add(nums[i]);
                    tmp.add(nums[left]);
                    tmp.add(nums[right]);
                    res.add(tmp);
                    while (left < right && nums[left] == nums[left+1]) left++;
                    while (left < right && nums[right] == nums[right-1]) right--;
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
}
```
</details>


