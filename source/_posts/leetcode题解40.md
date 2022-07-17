---
title: leetcode题解40：组合数II
date: 2022-05-21 11:22:22
tags:
    - leetcode
    - 数组
    - 深度优先遍历
    - 递归
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第40题](https://leetcode.cn/problems/combination-sum-ii/)
<!--more-->


## 分析

相比于39题的改变为两点：
1. 第7步中，由于现在数不可以重复取，则继续求解从`i+1`开始和为`target-candidates[i]`的路径
2. 由于`candidates`存在重复的数，表示第6步中，遍历`i: [index, len(candidates)]`；执行的操作应该加一步去掉重复的，即如果以`candidates[i]`数开始的情况已经求解过一遍了，那么后面再遇到与`candidates[i]`相同的数就不需要继续考虑了。


## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(candidates, target, combinations, temp, index):
            if target == 0:
                combinations.append(temp[:])
                return
            if target < 0 or index >= len(candidates):
                return
            i = index
            for i in range(index, len(candidates)):
                if i > index and candidates[i] == candidates[i-1]:
                    continue
                if target - candidates[i] < 0:
                    return
                temp.append(candidates[i])
                dfs(candidates, target - candidates[i], combinations, temp, i+1)
                temp.pop()

        combinations, temp = [], []
        candidates.sort()
        dfs(candidates, target, combinations, temp, 0)
        return combinations
```
</details>
