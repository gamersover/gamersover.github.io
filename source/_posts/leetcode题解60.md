---
title: leetcode题解60：第k个排列
date: 2022-08-03 13:51:42
tags:
    - leetcode
    - 数学
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第60题](https://leetcode-cn.com/problems/permutation-sequence/)

<!--more-->

## 分析

最简单的思路，就是按照[力扣第31题](https://caoqinping.com/2022/05/08/leetcode题解31/)的思路，每次求下一个排列，直到第$k$个。

但是有更高效的方法，我们知道如果首位是`1`，那么一共有`(n-1)!`种情况，所以如果首项是`2`，那么至少是`1 * (n-1)!`项之后了，一般地，如果首项是`i`，那么至少是`(i-1) * (n-1)!`项之后了。所以如果给定了`k`，则`(k-1) / (n-1)! + 1`就表示首项的数字，确定了首项，再使用类似的方法确定第二项，只不过这时数字应该去掉首项的数字后内选择，以此类推。整个公式可以写成：
$$
    k - 1 = a_n (n-1)! + a_{n-1} (n-2)! + \cdots + a_2 1! + a_1 1
$$

依次求出$a_i$，其表示的意思为剩下的数字按照大小排序后的`index`，即可得到答案。

1. 这里给出一个例子说明一些细节；比如`n=4, k=9`时，如何求解；首先需要计算到$(n-1)!$，即$0!=1, 1!=1, 2!=2, 3!=6$；剩下数字为`1 2 3 4`；
2. 然后计算$a_4=(k-1) / 3! = 1$，所以首项是`2`，剩下的数字为`1 3 4`；
3. 首项`2`确定后，即前面已经有$a_4 \cdot 3!=6$项了，那么就只要考虑`1 3 4`组成的排列的第`k-6=3`项了，同样的方法计算$a_3 = (3 - 1) / 2! = 1$，从而第二项是数字`3`；
4. 前两项为`23`后，剩下的数字为`1 4`，前面已经确定$a_4 3! + a_3 2! = 8$项， 剩下`1`项，然后计算`a_2= (1 - 1) / 1! = 0`，即第三项数字为`1`；
5. 最后一项只能是`4`；
6. 所以排列是`2 3 1 4`

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        fractal = [1]*n
        for i in range(1, n):
            fractal[i] = fractal[i-1] * i
        nlist = list(range(1, n+1))
        ans = ""
        for i in range(n):
            ind = (k-1) // fractal[n-i-1]
            k %= fractal[n-i-1]
            ans += str(nlist[ind])
            nlist.pop(ind)
        return ans
```
