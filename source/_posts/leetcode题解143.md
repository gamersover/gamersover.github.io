---
title: leetcode题解143：重排链表
date: 2023-11-11 11:39:39
tags:
    - 双指针法
    - 链表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第143题](https://leetcode.cn/problems/reorder-list)

<!--more-->

## 分析

这题本身不难，主要是考察两点：
1. 利用双指针法找到链表的中间节点
2. 链表的反转


具体步骤如下：
1. 首先找到链表的中间节点记为`mid`
2. 将后半部分反转
3. 将前半部分和后半部分拼接起来

其他的看代码理解即可

## 代码

```python
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return
        mid = self.find_middle_node(head)
        head2 = mid.next
        mid.next = None
        head2 = self.reverse_list(head2)
        self.merge_list(head, head2)

    def find_middle_node(self, head):
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def reverse_list(self, head):
        curr = head
        while curr and curr.next:
            temp = curr.next
            curr.next = temp.next
            temp.next = head
            head = temp
        return head

    def merge_list(self, head1, head2):
        while head1 and head2:
            temp1, temp2 = head1.next, head2.next
            head1.next = head2
            head2.next = temp1
            head1, head2 = temp1, temp2
```
