---
title: leetcode题解23：合并K个升序链表
date: 2021-05-05 10:33:06
tags:
    - leetcode
    - 链表
    - 优先队列
    - 最小堆
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第23题](https://leetcode-cn.com/problems/merge-k-sorted-lists)

给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。
<!--more-->
 
示例 1：

> 输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
    1->4->5,
    1->3->4,
    2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

示例 2：

> 输入：lists = []
输出：[]

示例 3：

> 输入：lists = [[]]
输出：[]


提示：

* `k == lists.length`
* `0 <= k <= 10^4`
* `0 <= lists[i].length <= 500`
* `-10^4 <= lists[i][j] <= 10^4`
* `lists[i]` 按 升序 排列
* `lists[i].length` 的总和不超过 `10^4`

## 分析
该题的想法应该很简单：利用归并排序的思想，利用`k`个指针分别指向`k`个链表的头，然后比较`k`个指针指向的数的大小，将最小的那个添加到结果链表中，然后将取得最小值的那个链表的指针指向下个结点，接着继续比较`k`个指针指向的数的大小，重复上述步骤。

该题关键在于是不是每次都需要重新比较`k`个数的大小，这样比较每次都要`o(klogk)`的时间复杂度。显然不是，对于一个已经排好序的数列，每次只需要进出一个数，这时可以考虑用最小堆实现的优先队列，该数据结构可以在排好序的数组中插入一个新的数，且时间复杂度降到`o(logk)`。关于最大堆最小堆的原理可以查看这篇博客[java数据结构：基于树的堆](https://blog.csdn.net/cetrol_chen/article/details/80377552)。

## 算法
1. 使用最小堆维护一个优先队列`pqueue`
2. 遍历所有链表的头指针，并添加到`pqueue`
3. 初始化结果伪链表头`fhead`和当前结点`curr=fhead`，进入循环体，直到`pqueue`为空
4. 令`node=pqueue.pop()`，并将`node`添加到结果链表中`curr->next = node`，更新`curr=curr->next`
5. 如果`node.next`不为空指针，则将`node.next`添加到`pqueue`中，回到第4步，并直到循环结束
6. 返回`fhead->next`

## 代码

<details open>
<summary>python3</summary>

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists):
        plist = []
        # 由于headq不能自定义比较器，所以需要一个序号i，在结点的val相同时，比较序号来排序
        i = 0
        for node in lists:
            if node is not None:
                heapq.heappush(plist, (node.val, i, node))
                i += 1

        fhead = ListNode()
        curr = fhead
        while len(plist) > 0:
            node = heapq.heappop(plist)[2]
            curr.next = node
            curr = curr.next

            if node.next is not None:
                heapq.heappush(plist, (node.next.val, i, node.next))
                i += 1

        return fhead.next
```
</details>


<details>
<summary>c++</summary>

```cpp
#include <queue>
using namespace std;

class Solution {
public:
    struct Item {
        ListNode *node;
        bool operator < (const Item &item) const {
            return node->val > item.node->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<Item> plist;
        for (auto node : lists) {
            if (node) plist.push({ node });
        }
        ListNode fhead, *curr = &fhead;
        while (!plist.empty()) {
            auto item = plist.top();
            plist.pop();
            curr->next = item.node;
            curr = curr->next;
            if (item.node->next) plist.push({ item.node->next });
        }
        return fhead.next;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
import java.util.PriorityQueue;

class Solution {
    class Item implements Comparable<Item>{
        ListNode node;

        Item(ListNode node){
            this.node = node;
        }

        public int compareTo(Item item2){
            return this.node.val - item2.node.val;
        }
    }

    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<Item> pqueue = new PriorityQueue<Item>();
        for (ListNode node : lists){
            if(node != null) pqueue.offer(new Item(node));
        }
        ListNode fhead = new ListNode(0);
        ListNode curr = fhead;
        while (!pqueue.isEmpty()){
            Item item = pqueue.poll();
            curr.next = item.node;
            curr = curr.next;

            if(item.node.next != null) pqueue.offer(new Item(item.node.next));
        }
        return fhead.next;
    }
}
```
</details>