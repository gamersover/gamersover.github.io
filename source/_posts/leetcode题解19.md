---
title: leetcode题解19：删除链表的倒数第N个结点
date: 2021-03-28 11:45:23
tags:
    - leetcode
    - 链表
    - 双指针
categories: 算法
mathjax: true
---

该题来自于[力扣第19题](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第`n`个结点，并且返回链表的头结点。
进阶：你能尝试使用一趟扫描实现吗？

<!--more-->

示例 1：
> 输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

示例 2：
> 输入：head = [1], n = 1
输出：[]

示例 3：
> 输入：head = [1,2], n = 1
输出：[1]
 

提示：
* 链表中结点的数目为`sz`
* 1 <= `sz` <= 30
* 0 <= `Node.val` <= 100
* 1 <= `n` <= sz

## 分析
想找到倒数第`n`个结点，就相当于找到第`sz - n + 1`个结点，但是`sz`不知道，需要一趟遍历整个链表才能获得，这样看来无论如何都需要两遍扫描；还记得找到中点结点的方法吗？快慢指针，一遍扫描即可；这里也可以用类似的想法；比如，现有一链表如下，要删除倒数第2个结点：
> 1 -> 2 -> 3 -> <font color='blue'>4</font> -> 5 -> <font color='red'>6</font>

如果我们能够让第一个指针`p1`指向4，那么就可以进行删除操作了；利用快慢指针的想法，第二个指针`p2`需要指向最后一个结点即6，所以两个指针相差2个结点，如果最开始让`p1`指向1，`p2`指向3，然后只需让`p1++,p2++`直到`p2`到达最后一个结点，这时`p1`自然达到了倒数第`n`个结点了。

所以先让`p2`指针往右走`n`步，达到第`n+1`个结点，然后`p1,p2`指针同时走动1步，直到`p2`指针指向`sz`，这时`p1`自然会指向`sz -n`，即要删除结点的父结点位置，然后就可以删除了。当然，还要考虑特殊情况：删除一个结点的时候的特殊处理。或者用一个虚假的头指向链表原有head，这样就不用考虑特殊情况了。

## 算法
采用双指针法，
1. 初始化`p1=head,p2=head`
2. 进入循环`p2 = p2->next`，直到`i==n`，如果`p2==null`，表明删除的是头结点，则让`head = p1->next`退出程序，或则进入3
3. 进入循环`p1 = p1->next, p2=p2->next`，直到`p2->next == null`，表明`p2`已到达最后一个结点，此时让`p1->next = p1->next->next`

## 代码
<details open>
<summary>python3</summary>

```python
class Solution:
    def removeNthFromEnd(self, head, n):
        p1, p2 = head, head
        i = 0
        while i < n and p2 is not None:
            p2 = p2.next
            i += 1
        
        if p2 is None:
            head = p1.next
            return head
        
        while p2.next is not None:
            p1 = p1.next
            p2 = p2.next        
        p1.next = p1.next.next

        return head
```
</details>


<details>
<summary>c++</summary>

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* p1 = head;
        ListNode* p2 = head;
        int i = 0;
        while(i < n && p2 != nullptr){
            p2 = p2->next;
            i++;
        }
        if(p2 == nullptr){
            head = head->next;
            return head;
        }

        while(p2->next != nullptr){
            p1 = p1->next;
            p2 = p2->next;
        }
        p1->next = p1->next->next;
        return head;
    }
};
```
</details>


<details>
<summary>java</summary>

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode p1 = head;
        ListNode p2 = head;
        int i = 0;
        while(i < n && p2 != null){
            p2 = p2.next;
        }
        if(p2 == null){
            head = head.next;
            return head;
        }
        while(p2.next != null){
            p1 = p1.next;
            p2 = p2.next;
        }
        p1.next = p1.next.next;
        return head;
    }
}
```
</details>