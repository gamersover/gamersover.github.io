---
title: leetcode题解147：对链表进行插入排序
date: 2023-11-18 11:03:58
tags:
    - 排序
    - 链表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第147题](https://leetcode.cn/problems/insertion-sort-list/)

<!--more-->

## 分析

每次插入排序时，将待插入的结点依次和已经排好序的结点比较，找到合适的插入位置，将待插入的结点插入到合适的位置。

注意几点细节：
* 由于链表只有next指向，所以比较的时候从前往后比。
* 为了插入相应结点，我们需要记录待插入的结点的前一个结点，已经插入位置的前一个结点。对于单向链表，大家应该注意到对于结点的删除，添加很多时候都需要用到结点的前一个结点。
* 由于head结点没有前一个结点，所以在处理head的时候有些麻烦，为了方便，构造一个虚拟结点即可（也是常用的方法）

## 代码

```python
class Solution:
    def insertionSortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head

        dummy = ListNode(0, head)
        curr = head
        while curr.next:
            prev = dummy
            while prev.next.val < curr.next.val:
                prev = prev.next
            if prev == curr:
                curr = curr.next
                continue
            node = curr.next
            curr.next = node.next
            node.next = prev.next
            prev.next = node
        return dummy.next

```