---
title: LeetCode解题报告（61）-- 237. Delete Node in a Linked List
tags:
  - LeetCode
abbrlink: 40666
date: 2019-09-27 16:35:22
---

## Problem

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Given linked list -- head = [4,5,1,9], which looks like following:

![img](https://assets.leetcode.com/uploads/2018/12/28/237_example.png)

<!-- more -->

**Example 1:**

```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

**Example 2:**

```
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
```

**Note:**

- The linked list will have at least two elements.
- All of the nodes' values will be unique.
- The given node will not be the tail and it will always be a valid node of the linked list.
- Do not return anything from your function.

------

## Analysis

&emsp;&emsp;这是一道非常经典的面试题目，要求是删除链表中的一个节点。通常来说，在一个链表中我们要删掉一个节点，就需要找到它上一个指针，这样才能够把被删除节点的上一个节点和下一个节点连起来。但是这道题目给出的指针就是需要被删除的节点。在你这样的题目设定下，我们无法获取前一个节点，不妨换个思路，我们把当前的这个情况变为我们熟悉的情况，那么只需要**交换当前节点和下一个节点的值**。这样被删除的节点实际上就成了下一个节点，符合我们常规的做法了。

------

## Solution

&emsp;&emsp;交换两个节点的值，然后是常规链表删除操作。

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
    void deleteNode(ListNode* node) {
        int next = node->next->val;
        node->val = next;
        ListNode* toDelete = node->next;
        node->next = toDelete->next;
        delete toDelete;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道面试时很常考的链表题目，其实非常简单，关键是要转换思路，巧妙地将不熟悉的情况转化为我们熟悉的情况。这道题目的分享到这里结束，欢迎大家评论、分享，谢谢您的支持！
