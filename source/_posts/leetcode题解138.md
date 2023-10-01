---
title: leetcode题解138：随机链表的复制
date: 2023-09-30 11:20:28
tags:
    - 链表
categories: 算法
mathjax: true
---

## 描述

该题来自于[力扣第138题](https://leetcode.cn/problems/copy-list-with-random-pointer)

<!--more-->

## 分析

没有random指针的时候，非常简单，直接复制当前节点，然后复制next节点，并将当前节点的next指针指向新的next节点；由于random指针随机指定，所以无法在一次遍历的情况下将random指针确定。

我们知道在第一次遍历的情况下可以确定next指针的指向，那么下一遍历的时候想要确定random指针的指向，我们就必须建立起新节点和原始节点之间的关系，由于第一次遍历的时候原始节点的next指针已经确定了，所以原始节点next节点似乎没有信息了，那就先将原始节点的next指针指向新节点，然后看看会有什么问题？

将原始节点curr的next指针指向新节点后，执行`curr.next.random = curr.random.next`，就确定了新节点的random指针指向了。看起来没什么问题，但是如果我们继续遍历下一个节点呢？发现原始节点的下一个节点已经丢失了，原因就是next指针重新指向了新节点了，这时我们可以利用新节点的random指针，在第一次遍历的时候，让新节点`newCurr`的`random`指针指向`curr.next`，就可以知道原始节点的next指针是什么了。


具体来说就是，每次遍历到新节点curr执行以下步骤，第一次遍历
```java
newCurr.next = new Node(curr.next.val)
newCurr.random = curr.next
curr.next = newCurr
curr = newCurr.random
newCurr = newCurr.next
```

第二次遍历
```java
nextCurr = curr.next.random
curr.next.random = curr.random.next
curr.next = null
curr = nextCurr
```

处理头节点时，可以记录下newHead，然后返回newHead即可。


## 代码

```python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        if head is None:
            return None

        newHead = Node(head.val)
        curr, newCurr = head, newHead
        while curr.next:
            newCurr.next = Node(curr.next.val)
            newCurr.random = curr.next
            curr.next = newCurr
            curr = newCurr.random
            newCurr = newCurr.next

        curr.next = newCurr

        curr = head
        while curr and curr.next:
            nextCurr = curr.next.random
            if curr.random:
                curr.next.random = curr.random.next
            else:
                curr.next.random = None

            curr = nextCurr
        return newHead
```
