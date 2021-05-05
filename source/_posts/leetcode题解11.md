---
title: leetcode题解11：盛最多水的容器
date: 2021-01-11 20:11:59
tags:
    - leetcode
    - 数组
    - 双指针
categories: 算法
mathjax: true
---

## 描述
该题来自于力扣[第11题](https://leetcode-cn.com/problems/container-with-most-water/)

给你`n`个非负整数`a1，a2，...，an`，每个数代表坐标中的一个点`(i, ai)`。在坐标内画`n`条垂直线，垂直线`i`的两个端点分别为`(i, ai)`和`(i, 0)`。找出其中的两条线，使得它们与`x`轴共同构成的容器可以容纳最多的水。

<!--more-->

说明：你不能倾斜容器。

示例 1：
> 输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例 2：
> 输入：height = [1,1]
输出：1

示例 3：
> 输入：height = [4,3,2,1,4]
输出：16

示例 4：
> 输入：height = [1,2,1]
输出：2
 

提示：
* `n = height.length`
* `2 <= n <= 3 * 104`
* `0 <= height[i] <= 3 * 104`

## 分析

给定横坐标`i,j`，则`(i,height[i])`与`(j,height[j])`构成的容器能容纳水的量为`min(height[i],height[j])*(j-i+1)`，记`k = min(height[i], height[j])`，`l = j-i+1`，目标使得`k * l`最大；`k`物理意义就是两个端点的小的那个，`l`的物理意义就是两个端点的距离，所以如果距离`l`减少的同时，`k`也在减少，那么目标值肯定变少了。根据该现象优化：
1. 首先选取最左与最右两个端点，此时`l`是最大的
2. 比较两个端点的高度，如果比较高的那个往内移动，这时`k`要么不改变，要么减少，而`l`也再减少，所以`k*l`只会变少
3. 由第2点可知，比较高的端点移动只会减少目标值，所以只需要移动低的那个端点即可


### 算法
显然采用双指针法，一个初始为指向最左，一个初始为指向最右：
1. 初始化`i=0,j=n-1`，及目标最大值为`m=min(height[i],height[j])*(j-i+1)`
2. 如果`height[i] < height[j]`，则左端点向内移动即`i++`；
3. 否则，右端点向内移动即`j--`
4. 重新计算当前容纳水的值`cm`，如果`cm > m`，更新目标最大值`m = cm`
5. 重复步骤2


## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def maxArea(self, height):
        n = len(height)
        i, j = 0, n-1
        m = 0
        while i < j:
            m = max(m, min(height[j], height[i]) * (j - i))

            if height[i] < height[j]:
                i += 1
            else:
                j -= 1
        return m
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int i = 0, j = n - 1;
        int m = 0;
        while(i < j) {
            int cm = min(height[i], height[j]) * (j - i);
            m = cm > m ? cm : m;
            if (height[i] < height[j]){
                i++;
            }
            else{
                j--;
            }
        }
        return m;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public int maxArea(int[] height) {
        int m = 0;
        int i = 0;
        int j = height.length - 1;
        while(i < j){
            m = Math.max(m, Math.min(height[i], height[j]) * (j - i));
            if(height[i] < height[j]) i++;
            else j--;
        }
        return m;
    }
}
```
</details>