---
title: LeetCode解题报告（431）-- 143. Reorder List
mathjax: true
date: 2021-12-22 11:50:37
---

## Problem

You are given the head of a singly linked-list. The list can be represented as:

```
L0 → L1 → … → Ln - 1 → Ln
```

*Reorder the list to be on the following form:*

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

**Constraints:**

- The number of nodes in the list is in the range `[1, 5 * 104]`.
- `1 <= Node.val <= 1000`

------

## Analysis

&emsp;&emsp;题目给出一个链表，要求调整链表的顺序，第一个元素指向倒数第一个元素，第二个元素指向倒数第二个元素。链表类的题目无非就是操作节点的指针，这里我们先找出规律。如果把每隔一个把元素抽取出来，实际上就是两条链表的合并，一条是从第一个节点到中间的节点，另一条是从最后一个节点到中间的节点。这样一看问题就简化了，两条链表的合并很容易，把原链表切成两部分也很容易，把后半部分翻转一下也很容易，所以这道题目就解决了。实际上就是由三个简单的步骤组合而成：

1. 中间切分链表；
2. 翻转后半部分；
3. 两个链表合并。

## Solution

&emsp;&emsp;这里实现有一个细节需要注意，在切分完链表后，对后半部分进行翻转，`prev`初始化为`nullptr`，这是因为希望后半部分翻转完之后，结尾能指向空，而不是中间的节点，这是为了便于后续merge的判断条件。

------

## Code

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    void reorderList(ListNode* head) {
        // find the middle of the list
        ListNode* slow = head, *fast = head;
        
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        
        // reverse the second half
        ListNode* prev = nullptr;
        ListNode* cur = slow;
        while (cur) {
            ListNode* temp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = temp;
        }
        //slow->next = nullptr;
        
        // merge two list
        ListNode* p1 = head, *p2 = prev;
        ListNode* temp = nullptr;
        while (p2->next) {
            temp = p1->next;
            p1->next = p2;
            p1 = temp;
            
            temp = p2->next;
            p2->next = p1;
            p2 = temp;
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题第一眼看上去比较复杂，但实际上就是由三个简单的步骤组合而成，并没有新增内容。这道题目的分享到这里，感谢你的支持！
