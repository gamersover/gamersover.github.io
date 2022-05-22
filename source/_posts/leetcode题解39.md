---
title: leetcode题解39：组合总数
date: 2022-05-21 10:26:20
tags:
    - leetcode
    - 数组
    - 深度优先遍历
    - 递归
categories: 算法
mathjax: true
---


## 描述

该题来自于[力扣第39题](https://leetcode.cn/problems/combination-sum/)
<!--more-->


## 分析

这题简单分析发现可以用递归解决，但是递归的写法怎么写比较难想，将整个过程推理下来，发现是个深度优先遍历问题，以`candidates = [2,3,6,7], target=7`为例，取第一个数`2`，然后求`[2,3,6,7]`中和为`7-2=5`的解，即转化为了一个更小的相同问题，具体构建过程为一棵树，记录树中满足条件的所有路径即可，具体做法：
1. `candidates`排序，使其递增；
2. 所有解的数组为`res`，当前记录路径为`tmp`，剩下的和为`target`，当前遍历的数在`candidates`中的`index`；递归函数则为求解`candidates[index:]`中满足和为`target`的路径。
3. 进入递归函数主体，判断
4. 如果剩余的和`target==0`，表示当前路径满足条件，将`tmp`加入到`res`中；
5. 如果`target<0`，表示当前路径已不满足，直接退出该函数，返回上一层
6. 如果`target>0`，则需要求解以`candidates[index:]`中每个元素为根的树的满足条件的路径，从而遍历`i: [index, len(candidates)]`；遍历时执行以下操作；
7. 如果`target-candidates]i]<0`，表示第一个数就已经超过了，由于数组是递增的，直接退出循环了，退出函数；否则将当前值`candidates[i]`加入到路径`tmp`中，由于数可以重复取，继续求解从`i`开始和为`target-candidates[i]`的路径，进入递归，当求解完毕后，在继续下次循环之前，需要回溯现场，将`candidates[i]`踢出`tmp`
8. 当第3步的递归结束后，`res`就是所有解了


## 算法

<details open>
<summary>python</summary>

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(candidates, target, combinations, tmp, index):
            if target < 0:
                return
            if target == 0:
                combinations.append(tmp[:])
                return
            for i in range(index, len(candidates)):
                if target - candidates[i] < 0:
                    return
                tmp.append(candidates[i])
                dfs(candidates, target - candidates[i], combinations, tmp, i)
                tmp.pop()

        combinations = []
        tmp = []
        candidates = sorted(candidates)
        dfs(candidates, target, combinations, tmp, 0)
        return combinations
```
</details>
