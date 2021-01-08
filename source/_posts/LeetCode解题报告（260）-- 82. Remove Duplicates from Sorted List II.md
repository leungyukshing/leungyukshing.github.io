---
title: LeetCode解题报告（260）-- 82. Remove Duplicates from Sorted List II
tags:
  - LeetCode
mathjax: true
date: 2021-01-09 00:20:51

---

## Problem

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list **sorted** as well*.

<!-- more -->

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg" alt="Example1" style="zoom:67%;" />

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

**Example 2:**

<img src="https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg" alt="Example2" style="zoom:67%;" />

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.

------

## Analysis

&emsp;&emsp;这道题要求我们从一个链表中移除重复的元素。在之前我们遇到过数组中移除重复的元素，这里从数组变成了链表。从删除的角度来说，链表是比较方便的。我们用两个指针，一个`pre`指针指向当前元素的前一个，另一个`cur`指针指向当前元素，两个指针的作用就是用来跳过重复的数字。例如在Example 1中，当`cur`指向第一个3的时候，`pre`指向2。此时通过判断相同元素`cur`一直往后到第二个3，这个时候`pre->next = cur->next`就完成了相同元素的移除。

------

## Solution

&emsp;&emsp;在判断是否需要移除时，通过判断`cur`是否和`pre->next`相等得知。在没有重复元素的情况下两者是相等的。

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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummy = new ListNode(-101);
        dummy->next = head;
        ListNode* pre = dummy;
        
        while (pre->next) {
            ListNode* current = pre->next;
            while (current->next && current->next->val == current->val) {
                current = current->next;
            }
            
            if (current != pre->next) {
                // skip duplicate element
                pre->next = current->next;
            } else {
                pre = pre->next;
            }
        }
        return dummy->next;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是数组或链表中移除重复元素的系列题，也给了我们很好的思考。对于同样的问题，使用数组和链表求解的思路完全不一样。总体上说数组以交换操作居多，而链表以删除操作为主。这道题这道题目的分享到这里，谢谢您的支持！
