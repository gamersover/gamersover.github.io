---
title: leetcode题解29：两数相除
date: 2022-01-06 10:56:06
tags:
    - leetcode
    - 二分法
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第29题](https://leetcode-cn.com/problems/divide-two-integers/)

给定两个整数，被除数`dividend`和除数`divisor`。将两数相除，要求不使用乘法、除法和`mod`运算符。

返回被除数`dividend`除以除数`divisor`得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2

<!--more-->

示例1:

> 输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = truncate(3.33333..) = truncate(3) = 3
示例2:

> 输入: dividend = 7, divisor = -3
输出: -2
解释: 7/-3 = truncate(-2.33333..) = -2


提示：

* 被除数和除数均为`32`位有符号整数。
* 除数不为`0`。
* 假设我们的环境只能存储`32`位有符号整数，其数值范围是`[−2^31, 2^31 − 1]`。本题中，如果除法结果溢出，则返回`2^31 − 1`。


## 分析
不能使用乘除计算两个数的除法，只能用加减了，考虑到正负只对最后结果的正负号有影响，所以可以先讨论被除数和除数都为正数的情况；而最简单的思路就是用除数循环加自身直到最后的值大于被除数，然后计算循环的次数就是商了。伪代码如下：
```
sum = divisor
result = 1
while sum < dividend:
    sum += divisor
    result++;
```

上面思路显然效率太低了，问题在于每次只加上`divisor`，如果每次加上`sum`自身，然后`result`也加上自身，这样就使得`sum = pow(2, result) * divisor`，这样`sum`会是指数级增加，比之前的线性增加快了太多；

但是问题来了，按照上面的算法，最终商会定位到`pow(2, n-1)`和`pow(2, n)`，`n`是满足`pow(2, n-1) * divisor < dividend < pow(2, n) * divisor`正整数，接下来如何定位到确定的商的，注意到最终的商必然是`pow(2, n-1) + k`，即`pow(2, n-1) + k = dividend / divisor`, 从而得到`k = (dividend - pow(2, n-1) * divisor) / divisor`，就是说`k`也是两个数的商，那么可以继续使用上面的思路，只不过这时候被除数变为了`dividend - pow(2, n-1) * divisor = dividend - sum`。伪代码也很简单：
```
func div(dividend, divisor):
    if (dividend < divisor):
        return 0
    sum = divisor
    result = 1
    while (sum + sum) < dividend:
        sum += sum
        reuslt += result
    return result + div(dividend - sum, divisor)
```
由于`sum+sum`可能会溢出，所以改为`sum < dividend - sum`就好。

最后无非是考虑边界了，由于int32的数值范围是`[-2^31, 2^31-1]`，所以如果把值转为正数，那么范围会变成`[0, 2^31]`，这时候`2^31`就会溢出，怎么办呢？其实这里有个小技巧，不都转为正数，而是都转为负数，因为转为负数后的范围是`[-2^31, 0]`，这样就不会溢出了。当然被除数与除数全部转为负数后，要注意代码中的大小关系与全转为正数时的不同。

当然如果全部使用`long long`长整型，就没有那么多边界的问题需要考虑了。但是题目应该是倾向于只使用`int`整型。


## 代码

<details open>
<summary>c++</summary>

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        // 当dividend和divisor为INT_MIN时，不能使用绝对值，因为会溢出
        if (dividend == 0) return 0;
        if (dividend == INT_MIN){
            if (divisor == -1) return INT_MAX;
            if (divisor == 1) return INT_MIN;
        }
        if (divisor == INT_MIN) {
            if (dividend == INT_MIN) return 1;
            return 0;
        }

        bool isPositive = true;
        if (dividend > 0) {
            dividend = -dividend;
            isPositive = !isPositive;
        }
        if (divisor > 0) {
            divisor = -divisor;
            isPositive = !isPositive;
        }

        int result = div(dividend, divisor);
        return isPositive ? result : -result;
    }

    int div(int dividend, int divisor){
        if (dividend > divisor) return 0;
        int sum = divisor;
        int result = 1;
        // 注意： 不能使用sum - dividend，可能会先计算-dividend，从而导致溢出
        while ((dividend - sum) <= sum){
            sum += sum;
            result += result;
        }
        return result + div(dividend - sum, divisor);
    }
};
```
</details>