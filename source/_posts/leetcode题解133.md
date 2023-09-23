---
title: leetcode题解133：克隆图
date: 2023-09-23 13:07:06
tags:
    - 深度优先遍历
    - 递归
    - 字符串
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第133题](https://leetcode.cn/problems/clone-graph/)

<!--more-->

## 分析

既然是图的遍历问题，几乎是用深度优先或者广度优先遍历，这里我们使用深度优先遍历。

先复制当前结点，然后查看所有邻居，如果邻居已经被复制了，直接添加到该节点的邻居节点数组中，否则就复制该邻居节点（又是一个相同的小问题，递归处理），并加到邻居数组中。

所以关键在于要维护一个map，记录已经复制过的节点。


## 代码

```python
class Solution:
    def clone(self, node):
        if node is None:
            return None

        else:
            head = Node(node.val)
            self.all_nodes[node] = head
            for inode in node.neighbors:
                if inode in self.all_nodes:
                    head.neighbors.append(self.all_nodes[inode])
                else:
                    nei = self.clone(inode)
                    head.neighbors.append(nei)
            return head

    def cloneGraph(self, node: 'Node') -> 'Node':
        self.all_nodes = dict()
        return self.clone(node)
```
