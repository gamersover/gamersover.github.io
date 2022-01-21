---
title: leetcode题解25：K个一组翻转链表
date: 2021-07-18 09:56:33
tags:
    - leetcode
    - 链表
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第25题](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

进阶：
你可以设计一个只使用常数额外空间的算法来解决此问题吗？
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

<!--more-->


示例 1：
> 输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]

示例 2：
> 输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]

示例 3：
> 输入：head = [1,2,3,4,5], k = 1
输出：[1,2,3,4,5]

示例 4：
> 输入：head = [1], k = 1
输出：[1]
提示：

列表中节点的数量在范围 sz 内
* 1 <= sz <= 5000
* 0 <= Node.val <= 1000
* 1 <= k <= sz


## 分析
思路比较直观，首先从当前节点开始往后数节点的个数，如果不超过`k`个，直接返回头节点；如果达到`k`个节点，则翻转这`k`个节点并返回翻转后的头节点，然后循环上述步骤就可以了。这里重点分析如何翻转`k`个节点，比如说翻转`1->2->3->4`的步骤如下：
1. 将2放到前面，`2->1->3->4`
2. 将3放到前面，`3->2->1->4`
3. 将4放到前面，`4->3->2->1`

所以依次仿照上述操作即可翻转任意`k`个节点了，假设已知`k`个节点的头指针`head`和尾指针`tail`，并且指针`curr`前面已经翻转，现在需要翻转`curr`后的节点，如下所示
```
head -> ... -> curr -> first -> ... -> 
```
`head`到`curr`已经翻转了，那么接下来需要将`first`放到前面即可，可以进行以下操作
1. `first = curr.next`
2. `curr.next = first.next`
3. `first.next = head`
4. 最后更新`head`方便下次循环，`head = first`

直到`head`与`tail`相等，表示`tail`已经在头部了，所以不需要再翻转了。

## 算法
1. 初始化`fakeHead`，并令`fakeHead.next = head`，初始`pre`，并令`pre = fakeHead`
2. 进入循环，循环条件是`head != null`
3. 判断从`head`开始是否有`k`个节点，如果没有进入步骤4，如果有进入步骤5
4. 由于不足`k`个节点，直接返回`fakeHead.next`，退出程序
5. 获取`k`个节点的头`head`和尾`tail`，并翻转这`k`个节点，得到新的头`head`和尾`tail`
6. 拼接翻转后的链表`pre.next = head`
7. 更新`pre`,`head`准备下次循环，`pre=tail`，`head=pre.next`，回到步骤2
8. 循环条件不满足退出并返回`fakeHead.next`


## 代码
<details open>
<summary>python3</summary>

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def reverse(self, head, tail):
        curr = head
        while head != tail:
            first = curr.next
            curr.next = first.next
            first.next = head
            head = first
        return head, curr

    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        fakeHead = ListNode(0, head)
        pre = fakeHead
        while head:
            tail = pre
            for _ in range(k):
                tail = tail.next
                if tail is None:
                    return fakeHead.next
            head, tail = self.reverse(head, tail)
            pre.next = head
            pre = tail
            head = pre.next
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
	void reverse(ListNode* &head, ListNode* &tail) {
		ListNode* curr = head;
		while (head != tail) {
			ListNode* first = curr->next;
			curr->next = first->next;
			first->next = head;
			head = first;
		}
		tail = curr;
	}
	ListNode* reverseKGroup(ListNode* head, int k) {
		ListNode* fakeHead = new ListNode(0, head);
		ListNode* pre = fakeHead;
		while (head) {
			ListNode* tail = pre;
			for (int i = 0; i < k; i++) {
				tail = tail->next;
				if (tail == nullptr) return fakeHead->next;
			}
			reverse(head, tail);
			pre->next = head;
			pre = tail;
			head = pre->next;
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
    public ListNode reverse(ListNode head, ListNode tail) {
        ListNode curr = head;
        while(head != tail) {
            ListNode first = curr.next;
            curr.next = first.next;
            first.next = head;
            head = first;
        }
        tail = curr;
        return tail;
    }

    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode fakeHead = new ListNode(0, head);
        ListNode pre = fakeHead;
        while (head != null) {
            ListNode tail = pre;
            for(int i = 0; i < k; i++) {
                tail = tail.next;
                if (tail == null) return fakeHead.next;
            }
            ListNode newTail = reverse(head, tail);
            pre.next = tail;
            pre = newTail;
            head = pre.next;
        }
        return fakeHead.next;
    }
}
```
</details>