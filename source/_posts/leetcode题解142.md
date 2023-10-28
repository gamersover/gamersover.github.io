---
title: leetcode题解142：环形链表 II
date: 2023-10-14 11:33:00
tags:
    - 双指针法
    - 链表
    - 数学
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第142题](https://leetcode.cn/problems/linked-list-cycle-ii)

<!--more-->

## 分析

判断是不是环形链表可以直接使用快慢指针法，当两个指针相遇的时候，肯定存在环，否则不存在环。

但此题是求解环的入口，最简单的方法用哈希表记录经过的结点，那么第一次出现两次的结点就是环的入口。可是有更优的方法，即使用`O(1)`的空间，这种方法怎么来，先经过一番数学推理。

假设环入口结点位置（从`0`开始的索引）为`x`，链表总长度为`n`，那么环的长度为`n - x`；再假设慢指针走了`y`步后与快指针相遇，那么有
$$
    \begin{aligned}
    & (y - x) \mod (n - x) = (2y - x) \mod (n - x) \\
    & \Rightarrow y \mod (n - x) = 0
    \end{aligned}
$$
而且可知慢指针或者快指针相遇的结点位置为
$$
    x + (y - x) \mod (n - y)
$$
观察式子，如慢指针继续走`x`步，此时慢指针的结点位置为
$$
    x + (y - x + x) \mod (n - y) = x
$$
即我们求的`x`，也就是说相遇的点继续走`x`步即可回到环的入口；还有哪个结点走`x`步可以到达环的入口？没错就是头结点。所以在快慢指针相遇后，头结点和慢指针同步走，直到相遇，就是环的入口。

## 算法

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None or head.next.next is None:
            return None

        meet_node = self.getMeetNode(head)

        if meet_node:
            entry = head
            while entry != meet_node:
                entry = entry.next
                meet_node = meet_node.next
            return entry
        else:
            return None

    def getMeetNode(self, head):
        fast, slow = head.next.next, head.next
        while fast is not None and fast.next is not None:
            if fast == slow:
                return slow
            else:
                fast = fast.next.next
                slow = slow.next
        return None
```