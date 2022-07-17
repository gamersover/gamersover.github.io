---
title: leetcode题解45：跳跃游戏 II
date: 2022-07-16 11:33:42
tags:
    - leetcode
    - 数组
    - 贪心
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第45题](https://leetcode.cn/problems/jump-game-ii/)
<!--more-->

## 分析

仔细想想，如果从当前位置$i$最远可以第$i+k$个位置，那么从当前位置跳到$i+1$到$i+k$位置的步数是一样的，比如`nums=[2,3,1,1,4]`，那么从$i=0$开始跳，最远可以跳跃到$i+nums[i]=2$个位置，即要到达`3 1`只需一步，那么写算法的时候，可以一次更新每个位置要达到的最小步数，但是显然可以继续优化；

有必要每次往后更新每个位置的步数吗？因为从位置`0`最远能跳到位置`2`，所以位置`1,2`都是`1`步到达，只有当超过位置`2`位置时，步数才去更新（+1），这样就好了；

此时考虑另一个问题，到达位置`1,2`的最小步数确定了，那后面的位置怎么确定呢？显然是位置`1,2`之中能跳到的最远距离了，所以遍历时，还需要计算`j+nums[j]`的最大值，即跳`2`步能达到的最大距离，其中`j`是跳`1`步能达到的最远距离。

## 算法
1. 首先令`step`为到达当前位置所需的步数，显然到达初始位置只需`0`步，从而设置初始`step=0`；令`currReach`为当前能达到的最远位置，显然设置初始`currReach=0`；令`nextReach`为下一个能达到的最远位置，同样初始`nextReach=0`；
2. 遍历数组`i in [0, n-1]`
3. 判断当前位置`i`是否超过了`currReach`，如果是，则执行第4步，否则执行第5步；
4. 超过了，那么`step=step+1`，且`currReach = nextReach`，即当前能到达的最远距离更新为下一次能到达的最远距离；
5. 没超过，继续找下一次能到达的最远距离，即更新`nextReach = max(nextReach, i+nums[i])`；
6. 遍历结束，返回`step`


## 代码

<details open>
<summary>c++</summary>
```c++
class Solution {
public:
	int jump(vector<int>& nums) {
		int n = nums.size();
		int currReach = 0;
		int maxReach = 0;
		int step = 0;
		for (int i = 0; i < n; i++) {
			if (i > canReach) {
				step++;
				currReach = maxReach;
			}
			maxReach = maxReach > nums[i] + i ? maxReach : nums[i] + i;
		}
		return step;
	}
};
```
