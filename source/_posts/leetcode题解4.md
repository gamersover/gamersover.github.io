---
title: leetcode题解4：寻找两个正序数组的中位数
date: 2020-11-14 09:41:06
tags:
    - leetcode
    - 数组
    - 二分法
categories: 算法
mathjax: true
---


## 描述

该题来自于[力扣第四题](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小为 m 和 n 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的中位数。
进阶：你能设计一个时间复杂度为 `O(log(m+n))` 的算法解决此问题吗？

<!--more-->

示例 1：

> 输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2

示例 2：

> 输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5

示例 3：

> 输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000

示例 4：

> 输入：nums1 = [], nums2 = [1]
输出：1.00000

示例 5：

> 输入：nums1 = [2], nums2 = []
输出：2.00000
 

提示：

> * nums1.length == m
> * nums2.length == n
> * 0 <= m <= 1000
> * 0 <= n <= 1000
> * 1 <= m + n <= 2000
> * -106 <= nums1[i], nums2[i] <= 106


## 分析
时间复杂度需达$O(\log(n))$级别，首先想到的是二分法，但该题如何使用二分求解，还需仔细分析。

### 中位数特点

中位数的特点是比它小的数和比它大的数一样多，如下面例子

> 1 2 3 4 <font color='blue'>5</font> 6 7 8 9

`5`的前面有四个数`1 2 3 4`，后面有四个数`6 7 8 9`，从而`5`是中位数

> 1 2 3 4 <font color='blue'>5 6</font> 7 8 9 10

`5 6`的前面有四个数`1 2 3 4`，后面有四个数`6 7 8 9`，从而`(5+6)/2=5.5`是中位数

所以想找到中位数就需要找到一个分割使得以下两点满足：
1. 该分割的左边元素比右边元素至多多出一个
2. 该分割的右边元素都要大于左边的元素

### 两个排序数组的中位数寻找

对于两个排序数组，比如

> 1 4 5 7 8 9
> 2 3 6

这时分割应该是

> <font color='red'>1 4 5</font> <font color='blue'>7 8 9</font>
> <font color='red'>2 3</font> <font color='blue'>6</font>

可以看到分割的左边为`1 4 5 2 3`，右边为`6 7 8 9`，满足上述两点。

所以基本想法是首先遍历所有满足第一点的分割，然后判断该分割是否满足第二点即可。

* 满足第1点的分割

    找分割无非就是如何划分，使得左边有5个数罢了，而这5个数来自于上面数组和下面数组，从而以`2 3 6`作为基准，如果`2 3 6`中没有划分到左边的，那么左边5个数都是来自上面数组的`1 4 5 7 8`；如果`2 3 6`中只有`2`划分到左边，那显然是上面数组的`1 4 5 7`划分到左边。
    所有分割情况如下四种：
    （1）

    > <font color='red'>1 4 5 7 8</font> <font color='blue'>9</font>
    > <font color='blue'>2 3 6</font>

    （2）

    > <font color='red'>1 4 5 7</font> <font color='blue'>8 9</font>
    > <font color='red'>2</font> <font color='blue'>3 6</font>

    （3）

    > <font color='red'>1 4 5</font> <font color='blue'>7 8 9</font>
    > <font color='red'>2 3</font> <font color='blue'>6</font>

    （4）

    > <font color='red'>1 4</font> <font color='blue'>5 7 8 9</font>
    > <font color='red'>2 3 6</font>

* 满足第2点的分割

   以下面分割为例

    > <font color='red'>1 4 5 7</font> <font color='blue'>8 9</font>
    > <font color='red'>2</font> <font color='blue'>3 6</font>

   要判断右边元素是否都要大于左边，只需判断`8 3`是否都要大于`7 2`，由于上下两个数组已经排好序了，表示$(8,7),(3,2)$已经满足条件了，所以只需比较$(8,2),(3,7)$是否满足条件，显然$3 < 7$，所以该分割不满足第2点。

### 最终算法
经过以上例子的分析，下面解释一般的情况，比如两个数组

> $a_0 a_1 a_2 a_3 a_4 a_5$
> $b_0 b_1 b_2$

假设任取分割$(4,1)$，表示分割点分别在$a_4$的左边和$b_1$的左边

> <font color="red">$a_0 a_1 a_2 a_3$</font> $a_4 a_5 a_6$
> <font color="red">$b_0$</font> $b_1 b_2$

这时根据$a_4,b_0$的大小关系和$b_1,a_3$的大小关系分成三种情况
1. $a_4 \ge b_0$且$b_1 \ge a_3$
   这时分割已经找到了
2. $b_1 < a_3$，可以推出$b_0 \le b_1 < a_3 \le a_4 \Rightarrow b_0 < a_4$
   这时比$b_1$大的数至少有$a_3,a_4,a_5,a_6,b_2$这5个数，显然比$b_1$还小的数不可能是中位数，所以分割点必然在$b_1$的右边
3. $a_4 < b_0$，可以推出$a_3 \le a_4 < b_0 \le b_1 \Rightarrow a_3 < b_1$
   这时比$b_1$小的数至少有$a_0,a_1,a_2,a_3,b_0$这5个数，显然比$b_1$还大的数不可能是中位数，所以分割点必然在$b_1$的左边

如此才出现了二分的样子了。 所以假设`nums2`是短的那个数组，且长度为`n`，取`0<=j<=n`作为`nums2`的分割点，如果`j=n`表示`nums2`的所有元素都归左边，若`j<n`，则`nums2[j]`左边的元素归左边。

由于二分法每次可以筛选掉一半，所以初始化`jleft=0,jright=n`，然后执行

1. `j = (jleft+jright) // 2`，显然此时`nums1`的分割点`i = (m+n+1)//2 - j`，如下

    > ... `nums1[i-1], nums1[i]`, ...
    > ... `nums2[j-1], nums2[j]`, ...

    这时需比较`nums1[i],nums2[j-1]`的大小以及`nums2[j],nums1[i-1]`的大小关系了，

2. 若`nums1[i] >= nums2[j-1] && nums2[j] >= nums1[i-1]`表示找到了分割点，此时若`m+n`是奇数，表示中位数只有一个，而分割的左边比右边多一个元素，所以中位数必然是`max(nums1[i-1], num2[j-1])`；如果`m+n`是偶数，中位数取两个的均值，此时取`max(nums1[i-1], num2[j-1])`和`min(nums1[i], num2[j])`的均值就是中位数了。**也就是说中位数要么是左边数的最大值，要么是左边数的最大值与右边数的最小值的均值。**

3. 若`nums2[j] < nums1[i-1]`，这时分割点在`j`的右边，所以令`jleft=j+1`，然后回到第1步
4. 若`nums1[i] < nums2[j-1]`，这时分给点在`j`的左边，所以令`jright=j-1`，然后回到第1步

按照上述步骤下去，就可以找到最终解了，记左边最大值为`max_left`，右边最小值为`min_right`；
最后注意边界:

边界1：若`j=0,i<m`，此时如下

> ...,`nums1[i-1],nums1[i]`,...
> `nums2[0],nums2[1]`,...

可知左边只有`nums1[i-1]`，所以`max_left=nums1[i-1]`；而`min_right=min(nums[i],nums2[k])`；

边界2：若`j=0,i=m`，此时如下

> ...,`nums1[m-1]`
> `nums2[0],nums2[1]`,...

可知`max_left=nums1[m-1]`，`min_right=nums2[0]`;

边界3：若`j=n,i>0`，此时如下

> ...,`nums1[i-1],nums1[i]`,...
> ...,`nums2[n-2],nums2[n-1]`

`max_left=max(nums1[i-1],nums2[n-1])`；`min_right=nums1[i]`;

边界3：若`j=n,i=0`，此时如下

> `nums1[0],nums1[1]`,...
> ...,`nums2[n-2],nums2[n-1]`

`max_left=nums2[n-1]`；`min_right=nums1[0]`。

由于每次都是对最短的数组进行二分，所以时间复杂度为$O(\log(\min(m, n)))$。

## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        m, n = len(nums1), len(nums2)
        if m < n:
            m, n, nums1, nums2 = n, m, nums2, nums1

        half_len = (m + n + 1) >> 1
        jleft, jright = 0, n
        while jleft <= jright:
            j = (jleft + jright) >> 1
            i = half_len - j
            if j < n and nums2[j] < nums1[i-1]:
                jleft = j + 1
            elif j > 0 and nums1[i] < nums2[j-1]:
                jright = j - 1
            else:
                break

        if j == 0:
            max_left = nums1[i-1]
        elif j == n and i == 0:
            max_left = nums2[j-1]
        else:
            max_left = max(nums1[i-1], nums2[j-1])

        if (m + n) % 2 == 1:
            return max_left

        if j == n:
            min_right = nums1[i]
        elif j == 0 and i == m:
            min_right = nums2[j]
        else:
            min_right = min(nums1[i], nums2[j])

        return (max_left + min_right) / 2
```
</details>


<details>
<summary>c++</summary>

```cpp
#include<vector>
using namespace std;

class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() < nums2.size()) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.size(), n = nums2.size();
        int halfLen = (m + n + 1) >> 1;
        int jleft = 0, jright = n;
        int i, j;
        while (jleft <= jright) {
            j = (jleft + jright) >> 1;
            i = halfLen - j;
            if (j < n && nums2[j] < nums1[i - 1])
                jleft = j + 1;
            else if (j > 0 && nums1[i] < nums2[j - 1])
                jright = j - 1;
            else break;
        }

        double max_left = 0, min_right = 0;
        if (j == 0) max_left = nums1[i - 1];
        else if (j == n && i == 0) max_left = nums2[j - 1];
        else max_left = max(nums1[i - 1], nums2[j - 1]);

        if ((m + n) % 2 == 1) return max_left;

        if (j == n) min_right = nums1[i];
        else if (j == 0 && i == m) min_right = nums2[j];
        else min_right = min(nums1[i], nums2[j]);

        return (min_right + max_left) / 2;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length < nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length, n = nums2.length;

        int halfLen = (m + n + 1) >> 1;
        int jleft = 0, jright = n;
        int i = 0, j = 0;
        while(jleft <= jright) {
            j = (jleft + jright) >> 1;
            i = halfLen - j;
            if (j < n && nums2[j] < nums1[i-1]) jleft = j + 1;
            else if(j > 0 && nums1[i] < nums2[j-1]) jright =  j - 1;
            else break;
        }

        double maxLeft, minRight;
        if (j == 0) maxLeft = nums1[i-1];
        else if(j == n && i==0) maxLeft = nums2[j-1];
        else maxLeft = Math.max(nums1[i-1], nums2[j-1]);

        if ((m + n) % 2 == 1) return maxLeft;

        if (j == n) minRight = nums1[i];
        else if(j == 0 && i == m) minRight = nums2[j];
        else minRight = Math.min(nums1[i], nums2[j]);

        return (maxLeft + minRight) / 2;
    }
}
```
</details>