---
title: leetcode题解22：括号生成
date: 2021-04-05 14:54:04
tags:
    - leetcode
    - 递归
    - 回溯
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第22题](https://leetcode-cn.com/problems/generate-parentheses/)

数字`n`代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且有效的括号组合。
<!--more--> 

示例 1：
> 输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]

示例 2：
> 输入：n = 1
输出：["()"]


提示：
* `1 <= n <= 8`

## 分析
要生成有效的括号，第一个字符必然是左括号`(`，那么第二个有可能是`(`或者`)`，以`n=3`为例，生成有效括号的过程应该如下：
<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/leetcode/No1.jpg" width="25%">

可以看出是一颗二叉树，二叉树的每条路径对应着一个有效括号组合，所以关键在于如何生成这颗树。思路也很简单，第一层已经确定只能是`(`，由于`n=3`表示最多可以有`3`个`(`，所以第二层可能是`(`，也可能是`)`，那第三层如何确定呢？

假如第二层选择的是`(`，由于左括号的个数为2，小于`n`，所以第三层也可能是`(`或者`)`；如果第二层选择的是`)`，一样的分析。

第三层如果选择的是`(`，这是左括号的个数已经等于`n`，所以第四层只能添加`)`了。

综上分析可知，假设当前层已有的左括号个数为`left`，已有右括号个数为`right`，若`left<n`，则当前层可以为`(`，同时若`right<left`，那么当前层也可以是`)`；否则结束，所有括号添加完毕。

树生成后，可以使用深度优先遍历所有的路径得到结果，这里可以采用递归的写法，注意一点就是遍历到叶子结点后，需要回溯到初始状态，具体可以看算法说明。

## 算法
1. 初始化`n`,`left=right=0`，以及路径`s=""`
2. 递归函数`f(n, left, rihgt, s)`
3. 进入递归主体，如果`len(s)==2*n`，表明`s`已经包含了所有括号，添加到结果列表中，并退出函数
4. 如果`left < n`，这时添加`(`，进入函数`f(n, left+1, right, s+"(")`，这里`s+"("`写在函数形参内，刚好避免回溯时`s`不受影响，`s`的变化至于该路径的前面所有层有关。
5. 如果`right < n`，这时添加`)`，进入函数`f(n, left, right, s+")")`

具体算法执行步骤可以这么理解：
首先一直添加`(`直到无法添加为止，然后添加`)`直到无法添加为止，即一直遍历树的左节点，然后开始回溯，这时`s`为`((()))`，根据算法会回溯到`s="(("`即第二层的左节点也是步骤4的结尾，之后进入步骤5。依次下去。

## 代码
<details open>
<summary>python3</summary>

```python
class Solution:
    def generateParenthesis(self, n):
        self.n = n
        self.result = []
        self.generate(0, 0)
        return self.result

    def generate(self, left, right, s=""):
        if len(s) == self.n * 2:
            self.result.append(s)
            return 
        
        if left < self.n:
            self.generate(left+1, right, s+"(")
        
        if right < left:
            self.generate(left, right+1, s+")")
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
vector<string> arr;
public:
    vector<string> generateParenthesis(int n) {
        generate(0, 0, "", n);
        return arr;
    }

    void generate(int left, int right, string s, int n){
        if(s.size() == 2 * n) {
            arr.push_back(s);
            return;
        }
        if(left < n){
            generate(left+1, right, s+"(", n);
        }
        if (right < left){
            generate(left, right+1, s+")", n);
        }
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        generate(n, 0, 0, "");
        return res;
    }

    void generate(int n, int left, int right, String s){
        if (s.length() == 2*n) {
            res.add(s);
            return;
        };
        
        if (left < n){
            generate(n, left+1, right, s+"(");
        }

        if (right < left){
            generate(n, left, right+1, s+")");
        }
    }
}
```
</details>