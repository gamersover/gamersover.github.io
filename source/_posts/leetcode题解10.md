---
title: leetcode题解10：正则表达式匹配
date: 2020-12-05 09:56:12
tags:
    - leetcode
    - 字符串
    - 正则表达式
    - 动态规划
categories: 算法
mathjax: true
---

## 描述
该题来自于力扣[第10题](https://leetcode-cn.com/problems/regular-expression-matching)

给你一个字符串`s`和一个字符规律`p`，请你来实现一个支持`'.'`和`'*'`的正则表达式匹配。

`'.'`匹配任意单个字符
`'*'`匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖**整个**字符串`s`的，而不是部分字符串。

<!--more-->

 
示例 1:

> 输入：s = "aa" p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。

示例 2:

> 输入：s = "aa" p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。

示例 3：

> 输入：s = "ab" p = ".\*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。

示例 4：

> 输入：s = "aab" p = "c\*a\*b"
输出：true
解释：因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。

示例 5:

> 输入：s = "mississippi" p = "mis\*is\*p\*."
输出：false


提示：

* `0 <= s.length <= 20`
* `0 <= p.length <= 30`
* `s` 可能为空，且只包含从 `a-z` 的小写字母。
* `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。
* 保证每次出现字符 `*` 时，前面都匹配到有效的字符


## 分析
* 该题可以采用状态机来解，类似的已在leetcode题解8中介绍了；该题还有其他解法——动态规划，用`dp[i][j]`来表示`s`的前`i`个字符与`p`的前`j`个字符是否匹配，那么可以注意到:
* 先不考虑`*`，如果`p[j]`与`s[i]`匹配到了，即要么`s[i]=p[j]`，要么`p[j]='.'`，那么`dp[i][j]=dp[i-1][j-1]`，否则就为`false`
* 再考虑`*`，即如果`p[j]='*'`
  * 如果`s[i]`与`p[j-1]`匹配
    * 则`*`前面的字符`p[j-1]`可能匹配`s`中多个，比如`s=aaa`，`p=a*`，这时`dp[i][j]=dp[i-1][j]`
    * 也可能`*`前面的字符不匹配`s`中的字符，比如`s=aa`，`p=aaa*`，这时`dp[i][j]=dp[i][j-2]`
  * 如果`s[i]`与`p[j-1]`不匹配，那么`*`前面的字符不匹配`s`中的字符，这时`dp[i][j]=dp[i][j-2]`

## 算法
根据前面分析知道，麻烦在于`p[j]='*'`时的处理，实际上无论`s[i]`与`p[j-1]`是否匹配到了，`*`前面的字符都可能不匹配`s`中的字符，也就是说`dp[i][j] = dp[i][j-2]`或者`dp[i][j] = dp[i][j-2] | dp[i-1][j]`；
所以实现的时候可以让`dp[i][j]`初始化为`false`，按照`dp[i][j]`的定义，`i,j`都必须大于0，这时可以让`dp[i][j]`表示两个空字符串是否匹配，当然设为`true`了，最后按照方程进行状态转移就可以了


## 代码

<details open>
<summary>python3</summary>

```python
class Solution:
    def match(self, schar, pchar):
        if pchar == ".":
            return True
        return pchar == schar

    def isMatch(self, s, p):
        m, n = len(s), len(p)
        dp = [[False]*(n+1) for _ in range(m+1)]
        # 空字符比较设为匹配，最终结果为dp[m][n]
        dp[0][0] = True
        for i in range(m+1):
            for j in range(1, n+1):
                if p[j-1] != '*':
                    if i > 0 and self.match(s[i-1], p[j-1]):
                        dp[i][j] |= dp[i-1][j-1]
                else:
                    # 末位是'*'，则先与上`*`前面的字符不匹配`s`中字符的情况，
                    # 比如s=aa p=aaa*或s=ab p=abc*
                    dp[i][j] |= dp[i][j-2]
                    # 然后判断`s[i]`与`p[j-1]`匹配且`*`前面的字符匹配`s`中多个的情况
                    if i > 0 and self.match(s[i-1], p[j-2]):
                        dp[i][j] |= dp[i-1][j]
        return dp[m][n]
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    bool match(char ch1, char ch2) {
        if (ch2 == '.') return true;
        return ch1 == ch2;
    }
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, false));
        dp[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j - 1] != '*') {
                    if (i > 0 && match(s[i - 1], p[j - 1]))
                        dp[i][j] |= dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] |= dp[i][j - 2];
                    if (i > 0 && match(s[i - 1], p[j - 2]))
                        dp[i][j] |= dp[i - 1][j];
                }
            }
        }
        return dp[m][n];
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    private boolean match(char ch1, char ch2){
        if(ch2 == '.') return true;
        return ch1 == ch2;
    }
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];

        dp[0][0] = true;
        for(int i=0; i<m+1; i++){
            for(int j=1; j<n+1; j++){
                if(p.charAt(j-1) != '*'){
                    if (i > 0 && match(s.charAt(i-1), p.charAt(j-1)))
                        dp[i][j] |= dp[i-1][j-1];
                }
                else{
                    dp[i][j] |= dp[i][j-2];
                    if (i > 0 && match(s.charAt(i-1), p.charAt(j-2)))
                        dp[i][j] |= dp[i-1][j];
                }
            }
        }
        return dp[m][n];
    }
}
```
</details>