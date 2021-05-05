---
title: leetcode题解5：最长回文子串
date: 2020-11-21 14:50:53
tags:
    - leetcode
    - 字符串
    - 动态规划
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第五题](https://leetcode-cn.com/problems/longest-palindromic-substring)

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

<!--more-->

示例 1：
> 输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

示例 2：
> 输入: "cbbd"
输出: "bb"


## 分析
关键思路是如何尽量少的遍历？注意到要判断$a_1a_2a_3 \cdots a_{n-1} a_n$是否是回文串，等价于判断$a_2a_3 \cdots a_{n-1}$是否是回文串，而且$a_1=a_n$是否成立，如果两个都成立，那么原字符串是回文串，否则就不是；所以解法思路是动态规划，
状态：`dp[i][j]`表示下标从`i`到`j`构成的子串是否是回文串
状态转移方程：`dp[i][j] = dp[i+1][j-1] && a[i]==a[j]`


## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def longestPalindrome(self, s):
        n = len(s)
        dp = [[False for i in range(n)] for j in range(n)]
        max_len = 0
        start, end = 0, 0
        for i in range(n-1, -1, -1):
            for j in range(i, n):
                if i == j:
                    dp[i][j] = True
               
                elif s[i] == s[j]:
                    if j - i > 1:
                        dp[i][j] = dp[i+1][j-1]
                    else:
                        dp[i][j] = True
                        
                else:
                    dp[i][j] = False
                    
                if dp[i][j] and max_len < j - i + 1:
                    start = i
                    end = j + 1
                    max_len = j - i + 1 
        
        return s[start:end]
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        int maxLen = 0;
        int start = 0, end = 0;
        vector<vector<int>> dp(n, vector<int>(n, false));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (i == j) dp[i][j] = true;
                else if (s[i] == s[j]) {
                    if ((j - i) > 1) dp[i][j] = dp[i + 1][j - 1];
                    else dp[i][j] = true;
                }
                else
                    dp[i][j] = false;

                if (dp[i][j] && (j-i+1 > maxLen)) {
                    maxLen = max(j - i + 1, maxLen);
                    start = i;
                    end = j;
                }
            }
        }
        return s.substr(start, maxLen);
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        int maxLen = 0;
        int start = 0, end = 0;
        Boolean[][] dp = new Boolean[n][n];
        for(int i=n-1; i>=0; i--) {
            for(int j=i; j<n; j++) {
                if(i == j) dp[i][j] = true;
                else if(s.charAt(i) == s.charAt(j)){
                    if(j - i > 1) dp[i][j] = dp[i+1][j-1];
                    else dp[i][j] = true;
                }
                else dp[i][j] = false;

                if (dp[i][j] && ((j - i + 1) > maxLen)){
                    maxLen = j - i + 1;
                    start = i;
                    end = j;
                }
            }
        }
        return s.substring(start, end+1);
    }
}
```
</details>