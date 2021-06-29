---
title: LeetCode解题报告（346) -- 86. Partition List
tags:
  - LeetCode
mathjax: true
abbrlink: 61202
date: 2021-04-20 22:02:11
---

## Problem

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

**Example 2:**

```
Input: head = [2,1], x = 2
Output: [1,2]
```

**Constraints:**

- The number of nodes in the list is in the range `[0, 200]`.
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`

------

## Analysis

&emsp;&emsp;题目给出一个链表，还有一个指定的值`x`，要求所有比`x`大的节点都要放在比`x`小的节点的后面，而且需要保持原有的相对位置。题目很好理解，实际上就是把原来的链表拆成两条，一条是比`x`小的节点组成的，另一个条是比`x`大的节点还有`x`节点本身组成的。最后再把第二条链表接到第一条链表后面即可。

------

## Solution

&emsp;&emsp;无

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
    ListNode* partition(ListNode* head, int x) {
        ListNode* dummy = new ListNode(-1);
        ListNode* dummy2 = new ListNode(-1);
        dummy->next = head;
        ListNode* p = dummy;
        ListNode* p2 = dummy2;
        while (p->next) {
            if (p->next->val < x) {
                p = p->next;
            } else {
                ListNode* temp = p->next;
                p->next = temp->next;
                temp->next = NULL;
                p2->next = temp;
                p2 = p2->next;
            }
        }
        
        p->next = dummy2->next;
        return dummy->next;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，属于简单的链表操作。这道题目的分享到这里，感谢你的支持！
