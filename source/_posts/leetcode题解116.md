---
title: leetcode题解116：填充每个节点的下一个右侧节点指针
date: 2022-11-24 15:31:19
tags:
    - leetcode
    - 递归
    - 二叉树
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第116题](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

<!--more-->

## 分析

最简单的思路就是层次遍历，然后每层连接就好了。这里介绍另一种方法。

树类问题一般是大问题化解小问题，该题也是如此，但是稍微有些不同，首先这么看，对于某个结点`root`，首先连接左右节点，即`root.left.next = root.right`，如果用这种思路递归的话，会少一些连线，如下图所示：
```
       1
     /   \
   2 ----> 3
  /  \    /  \
 4 -> 5  6 -> 7
```

1. `1`作为`root`结点，将`2,3`连接；
2. `2`作为`root`结点，将`4,5`连接
3. `3`作为`root`结点，将`6,7`连接

可以看到完成后，`5`和`6`之间少一个连线，这时回到以`2`作为`root`结点的情况，因为`2`有`next`，若将`root.right.next = root.next.left`，不就将`5,6`连起来了嘛。

所以递归时，做两件事：
1. `root.left.next = root.right`
2. `root.right.next = root.next.left`

再递归左右子树就行。


## 代码

```python
class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if root is None:
            return
        if root.left is not None:
            root.left.next = root.right
            if root.next is not None:
                root.right.next = root.next.left
        self.connect(root.left)
        self.connect(root.right)
        return root
```