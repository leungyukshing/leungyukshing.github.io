---
title: LeetCode解题报告（285）-- 141. Linked List Cycle
tags:
  - LeetCode
mathjax: true
abbrlink: 30257
date: 2021-02-09 20:48:19
---

## Problem

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that pos is not passed as a parameter**.

Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

 

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

------

## Analysis

&emsp;&emsp;题目给出一个链表，要求判断是否存在环。链表中是否存在环这个问题一般都睡通过快慢指针来解决的，慢指针一次走一步，快指针一次走两步，如果两个指针有相遇，说明存在环。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* p1 = head, *p2 = head;
        while (p2 && p2->next) {
            p1 = p1->next;
            p2 = p2->next->next;
            if (p1 == p2) {
                return true;
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;在链表中使用快慢指针是非常常见的解题思路，也是后续解决许多同类型题目的基础。这道题这道题目的分享到这里，谢谢您的支持！
