---
title: leetcode题解146：LRU缓存
date: 2023-11-12 12:57:44
tags:
    - 哈希表
    - 链表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第146题](https://leetcode.cn/problems/lru-cache/)

<!--more-->

## 分析

如果只是普通的put和get，那么哈希表就可以解决，关键在于需要对key进行更新，当key有操作时应该在排序在最前，涉及到O(1)复杂度的链表节点位置前置。

链表节点前置，很简单，就是在链表中删除它，然后将它置到头部即可。节点删除都应该非常熟悉了，单向链表只需要将当前节点的前一个节点的next指向当前节点的next即可。

为了O(1)时间复杂度，我们需要同时记住当前节点的前一个节点和下一个节点，所以这里就可以采用双向链表了。

有了双向链表这个问题就很简单了。还有一个问题就是用O(1)的时间复杂度判断节点是否在双向链表中，这里就需要一个哈希表（字典）。

步骤：
1. 实现一个双向链表，包含添加节点，删除节点
3. 结合哈希表实现get和put方法

## 代码
```python
class ListNode:
    def __init__(self, key=0, val=0, prev=None, next=None):
        self.key = key
        self.val = val
        self.prev = prev
        self.next = next


class DoubleLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def add(self, node):
        if self.head is None:
            self.head = node
            self.tail = node
        else:
            self.tail.next = node
            node.prev = self.tail
            self.tail = node

    def remove(self, node):
        if node.prev is not None:
            node.prev.next = node.next
        if node.next is not None:
            node.next.prev = node.prev
        if self.head == node:
            self.head = node.next
        if self.tail == node:
            self.tail = node.prev
        node.prev = None
        node.next = None

    def remove_head(self):
        node = self.head
        self.remove(node)
        return node

    def __str__(self):
        ret = []
        node = self.head
        while node is not None:
            ret.append("({},{})".format(node.key, node.val))
            node = node.next
        return "->".join(ret)

    def __repr__(self) -> str:
        return self.__str__()


class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.size = 0
        self.cache = dict()
        self.linked_list = DoubleLinkedList()


    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self.linked_list.remove(node)
            self.linked_list.add(node)
            return node.val
        return -1


    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.val = value
            node.key = key
            self.linked_list.remove(node)
            self.linked_list.add(node)
        else:
            node = ListNode(key, value)
            self.cache[key] = node
            self.linked_list.add(node)
            self.size += 1
            if self.size > self.cap:
                head = self.linked_list.remove_head()
                del self.cache[head.key]
                self.size -= 1
```