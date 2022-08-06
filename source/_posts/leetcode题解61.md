---
title: leetcode题解61：旋转链表
date: 2022-08-06 13:23:54
tags:
    - leetcode
    - 链表
---

## 描述

该题来自于[力扣第61题](https://leetcode-cn.com/problems/rotate-list/)

<!--more-->

## 分析

思路非常简单，将首尾相连，然后将`head`指向第`n-k`个结点的`next`结点，最后将第`n - k`个结点的`next`指向`NULL`；当然注意到`k`可能大于`n`，对令`k = k % n`就好了；比如下面这个例子：
```
1 -> 2 -> 3 -> 4 -> 5
```
当`k=1`时，首先首尾相连得到
```
1 -> 2 -> 3 -> 4 -> 5
^                   |
|-------------------
```
然后找到第4个结点，并将`head`指向该结点的`next`结点，然后将该结点的`next`指向`NULL`
```
1 -> 2 -> 3 -> 4  5 <-head
^                 |
|-----------------
```
这样链表不就变为了
```
5 -> 1 -> 2 -> 3 -> 4
```

## 代码
<details open>
<summary>python</summary>

```python
class Solution:
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if head is None or k == 0:
            return head

        last_node = head
        num_nodes = 1
        while last_node.next != None:
            last_node = last_node.next
            num_nodes += 1

        last_node.next = head
        for _ in range(num_nodes-k % num_nodes):
            last_node = last_node.next

        head = last_node.next
        last_node.next = None
        return head
```
