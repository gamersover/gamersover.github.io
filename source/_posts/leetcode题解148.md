---
title: leetcode题解148：排序链表
date: 2023-11-25 11:32:38
tags:
    - 排序
    - 链表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第148题](https://leetcode.cn/problems/sort-list/description/)

<!--more-->

## 分析

要 `O(nlogn)` 时间复杂度和常数级空间复杂度下完成排序，考虑快排，堆排，归并排序；考虑到数据结构是链表，所以归并排序会简单许多。

还是老套路，首先使用双指针找到中间节点，然后排序左边和右边，最后左右归并。

注意链表的老三样：双指针，节点的前一个节点，伪头节点。

为了递归，我们其实找到的是中间节点的前一个节点node，这样node.next就是中间节点，也就是右边链表的头节点，然后再让`node.next=null`，那左边链表的头节点还是head；这样左右都变成了子问题。

最后左右归并的时候，我们可以使用一个伪头节点，代码写起来更加舒服了。

## 代码

```python
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        mid = self.find_middle(head)
        second = mid.next
        mid.next = None
        first = self.sortList(head)
        second = self.sortList(second)
        new_head = self.merge(first, second)
        return new_head


    def find_middle(self, head):
        fast, slow = head.next.next, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def merge(self, head1, head2):
        fake_head = ListNode(0)
        curr = fake_head
        while head1 is not None and head2 is not None:
            if head1.val < head2.val:
                curr.next = head1
                head1 = head1.next
            else:
                curr.next = head2
                head2 = head2.next
            curr = curr.next

        if head1 is None:
            curr.next = head2
        else:
            curr.next = head1
        return fake_head.next
```