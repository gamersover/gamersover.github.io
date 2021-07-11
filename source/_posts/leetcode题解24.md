---
title: leetcode题解24
date: 2021-07-11 11:38:35
tags:
    - leetcode
    - 链表
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第24题](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

<!--more-->
示例1：
> 输入：head = [1,2,3,4]
输出：[2,1,4,3]

示例2：
> 输入：head = []
输出：[]

示例3：
> 输入：head = [1]
输出：[1]

提示：
* 链表中节点的数目在范围`[0, 100]`内
* `0 <= Node.val <= 100`


## 分析
假设当前节点`curr`以及两个相邻的节点指针`first, second`，其中`curr->first->second`，想要交换两个节点，只需要执行以下两步：
1. `first->next = second->next`
2. `second->next = first`

此时链表为`(curr,second)->first`，需再执行一步`curr.next = second`，链表才会变成`curr->second->first`

另外就是需要注意几种特殊情况：
1. 对于头部而言，如果`first = head`，那么`curr`就不存在，这时只需要设计一个`fakeHead`指针指向`head`即可
2. 如果`first==null`或者`second==null`，直接结束就可以，因为已经不需要交换了


## 算法
1. 初始一个`fakeHead`节点，使其指向链表头部`head`
2. 令`curr = fakeHead`
3. 进入循环，循环终止条件为`curr.next == null`或者`curr.next.next == null`
4. 令`first = curr.next`，`second = first.next`
5. 交换节点`first->next = second->next`，`second->next = first`，`curr.next = second`
6. 更新`curr = first`，并回到循环初始即步骤3
7. 循环退出，返回新的链表头节点`fakeHead.next`，程序结束


## 代码

<details open>
<summary>python3</summary>

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        fakeHead = ListNode(0, head)
        curr = fakeHead
        while curr.next and curr.next.next:
            first = curr.next
            second = first.next
            first.next = second.next
            second.next = first
            curr.next = second
            curr = first
        return fakeHead.next
```
</details>


<details>
<summary>c++</summary>

```cpp
struct ListNode {
	int val;
	ListNode *next;
	ListNode() : val(0), next(nullptr) {}
	ListNode(int x) : val(x), next(nullptr) {}
	ListNode(int x, ListNode *next) : val(x), next(next) {}
};
 
class Solution {
public:
	ListNode* swapPairs(ListNode* head) {
		ListNode* fakeHead = new ListNode(0, head);
		ListNode* curr = fakeHead;
		while ((curr->next != NULL) && (curr->next->next != NULL)) {
			ListNode* first = curr->next;
			ListNode* second = first->next;
			first->next = second->next;
			second->next = first;
			curr->next = second;
			curr = first;
		}
		return fakeHead->next;
	}
};
```
</details>


<details>
<summary>java</summary>

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 }
 
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode fakeHead = new ListNode(0, head);
        ListNode curr = fakeHead;
        while ((curr.next != null) && (curr.next.next != null)) { 
            ListNode first = curr.next;
            ListNode second = first.next;
            first.next = second.next;
            second.next = first;
            curr.next = second;
            curr = first;
        }
        return fakeHead.next;
    }
}
```
</details>