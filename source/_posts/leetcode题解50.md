---
title: leetcode题解50
date: 2022-07-23 15:52:29
tags:
    - leetcode
    - 递归
    - 快速幂
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第50题](https://leetcode.cn/problems/powx-n/)
<!--more-->

## 分析

一个非常简单的思路就是递归，当`n`是偶数时，计算`pow(x,n/2)*pow(x,n/2)`，则`n`是奇数时，计算`x * pow(x,(n-1)/2) * pow(x, (n-1)/2)`，一直递归直到`n`为1。

这里介绍另一种方法，使用迭代。考虑$x^n$时，将$n$表示成二进制$a_k \times 2^k + a_{k-1} \times 2^(k-1) + \cdots + a_0 \times 2^0$，从而
$$
    x^n = x^{a_k 2^k} \times x^{a_{k-1} 2^(k-1)} \times \cdots \times x^{a_0 2^0}
$$
其中$a_i (i=0,1,\cdots,k)$取值为$0$或$1$；

又因为$x^{2^k} = (x^{2^{k-1}})^2$，所以只需要依次计算$x^{2^i}$，当第$i$为二进制位$a_i=1$时，将$x^{2^i}$计入结果就好。

那$x^{10}$为例，$10$的二进制为$1010$，计算每个$x^{2^i}$，就是$x^8 \quad x^4 \quad x^2 \quad x$，二进制位为1的有$2^8 \quad 2^2$，所以$x^{10} = x^8 \times x^2$。

## 算法
1. 初始化结果为`ans = 1`，循环直到`n == 0`;
2. 判断`n`的末尾是不是1，如果是1，则执行第3步；
3. 记录结果，`ans *= x`
4. 更新`x`，`x *= x`
5. 将`n`右移一位

上述算法只考虑`n>=0`的情况，若`n<0`，则计算`1 / pow(x, -n)`即可


## 代码
<details open>
<summary>python</summary>
```python
class Solution:
    def mypow(self, x: float, n: int) -> float:
        ans = 1
        while n > 0:
            if n & 1:
                ans *= x
            x *= x
            n >>= 1
        return ans

    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            return 1 / self.mypow(x, -n)
        return self.mypow(x, n)
```