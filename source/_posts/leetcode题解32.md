---
title: leetcode题解32：最长有效括号
date: 2022-05-14 10:54:28
tags:
    - leetcode
    - 栈
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第32题](https://leetcode.cn/problems/longest-valid-parentheses/)
给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长度。

<!--more-->

示例 1：

> 输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"

示例 2：

> 输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"

示例 3：

> 输入：s = ""
输出：0


提示：

* 0 <= s.length <= 3 * 104
* s[i] 为 '(' 或 ')'


## 分析
注意嵌套括号也算，比如`())(())`的最长有效括号子串为`(())`。碰到括号匹配问题首先应该想到**栈**，由于存在嵌套，想要一次循环解决有点难，因为有些左括号必须到最后才知道是否有匹配的又括号，从而必须分两次解决，第一次循环通过压栈模拟获得每个匹配到的括号，如`())(()))()`，压栈过程（压栈记录索引，不需要记录元素）：
```
1. 栈内（从而栈底到栈顶）：0       出栈：null
2. 栈内（从而栈底到栈顶）：null    出栈：01
3. 栈内（从而栈底到栈顶）：2       出栈：01
4. 栈内（从而栈底到栈顶）：23      出栈：01
5. 栈内（从而栈底到栈顶）：234     出栈：01
6. 栈内（从而栈底到栈顶）：23      出栈：0145
7. 栈内（从而栈底到栈顶）：2       出栈：014536
8. 栈内（从而栈底到栈顶）：27      出栈：014536
9. 栈内（从而栈底到栈顶）：278      出栈：014536
10. 栈内（从而栈底到栈顶）：27      出栈：01453689
```
最终发现栈内为是`27`，为了计算方便可以在最后得到的栈内首位插入-1，末尾插入`len(s)`，这时栈内为`-1 2 7 10`，表示`0-1`，`3-6`，`8-9`都是连续括号，这样再循环一次就可以求得最长的连续括号子串了


## 算法


<details open>
<summary>python</summary>

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = [-1]
        for i in range(len(s)):
            if len(stack) > 1 and s[stack[-1]] == "(" and s[i] == ")":
                stack.pop()
            else:
                stack.append(i)
        stack.append(len(s))
        max_len = 0
        for i in range(1, len(stack)):
            curr_len = stack[i] - stack[i-1] - 1
            max_len = max(max_len, curr_len)
        return max_len

```
</details>

<details>
<summary>c++</summary>

```c++
class Solution {
public:
	int longestValidParentheses(string s) {
		vector<int> v;
		for (int i = 0; i < s.size(); i++) {
			if (!v.empty() && (s[v[v.size()-1]] == '(') && (s[i] == ')'))
				v.pop_back();
			else
				v.push_back(i);
		}
		int maxLen = 0;
		v.push_back(s.size());
		v.insert(v.begin(), -1);
		for (int i = 0; i < v.size() - 1; i++) {
			if (maxLen < v[i + 1] - v[i] - 1)
				maxLen = v[i + 1] - v[i] - 1;
		}
		return maxLen;
	}
};
```
</details>