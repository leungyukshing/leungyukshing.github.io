---
title: LeetCode解题报告（十）-- 2. Add Two Numbers
tags:
  - LeetCode
abbrlink: 6717
date: 2018-09-24 10:05:29
---
## Problem
You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
<!-- more -->

## Analysis
&emsp;&emsp;这题的设计的知识点是链表的操作和大数加法。使用链表可以存储很大的整数，这突破了`int`类型的限制。解题思路是使用大数加法，逐位相加，通过进位的方式来计算出每一位的值。难点是遍历两个链表（长度可能不一样），且链表中存储的数据是倒叙的，即从低位到高位。

## Solution
&emsp;&emsp;链表倒叙存储数据给我们的计算提供了方便，使得我们一开始就可以从地位开始计算，用`add`记录每一位向高位的进位，然后存入下一位中。由于这样的算法，在最后一位中，我们有可能会多存一个0在高位，因此需要使用一个`prev`指针指向前一个节点，当最后一个节点（最高位）为0时，`prev`就是最后一位，即去除掉多余的一位。
&emsp;&emsp;需要特别注意当两条链长度不同的情况，需要使用三个while循环来计算，但里面的内容大部分都是一样的。

## Code
```C++
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* result = new ListNode(0);
        ListNode* temp = result;
        ListNode* prev;
        int digit;
        int add = 0;
        while (l1 != NULL && l2 != NULL) {
            int sum = l1->val + l2->val + add;
            digit = sum % 10;
            add = sum / 10;
            temp->val = digit;
            temp->next = new ListNode(add);
            prev = temp;
            temp = temp->next;
            l1 = l1->next;
            l2 = l2->next;
        }
        while (l1 != NULL) {
            int sum = l1->val + add;
            digit = sum % 10;
            add = sum / 10;
            temp->val = digit;
            temp->next = new ListNode(add);
            prev = temp;
            temp = temp->next;
            l1 = l1->next;
        }
        while (l2 != NULL) {
            int sum = l2->val + add;
            digit = sum % 10;
            add = sum / 10;
            temp->val = digit;
            temp->next = new ListNode(add);
            prev = temp;
            temp = temp->next;
            l2 = l2->next;
        }
        if (temp->val == 0) {
            prev->next = NULL;
        }
        return result;
    }
};
```
**运行时间：**约48ms，超过24.38%的CPP代码。

## Summary
&emsp;&emsp;这题本身的难度不大，链表的操作也仅仅只是遍历与创建，稍微有难度的是通过一个`prev`来剔除最后一个为0的节点。在算法方面，我对大数加法已经有了一定的熟悉程度，实现起来也不是很难，注意进位即可。本题的题析到这里，谢谢您的支持！
