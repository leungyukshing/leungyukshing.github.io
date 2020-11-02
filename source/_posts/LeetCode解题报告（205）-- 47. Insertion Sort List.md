---
title: LeetCode解题报告（205）-- 47. Insertion Sort List
tags:
  - LeetCode
mathjax: true
date: 2020-11-03 00:32:12

---

## Problem

Sort a linked list using insertion sort.

<!-- more -->

![Example](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list

**Algorithm of Insertion Sort:**

1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3. It repeats until no input elements remain.

**Example 1:**

```
Input: 4->2->1->3
Output: 1->2->3->4
```

**Example 2:**

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

------

## Analysis

&emsp;&emsp;这是一道链表的选择排序。这里也再一次给出了选择排序的思路：有一个原链表，有一个新的链表（其实是一个新的head），从原来的链表中，每次拿第一个元素出来，然后放到新链表中合适的位置，最后返回新的链表头即可。

&emsp;&emsp;实际上每个节点的空间都是复用原来的就可以了，所以不需要额外的空间，但是因为有一个新的头，为了方便起见，在前面的题解中我也提到过，可以使用一个dummy节点作为头节点，便于把处理的代码统一起来。

------

## Solution

&emsp;&emsp;算法部分没有特别难的地方，按照选择排序的一般做法即可，小心处理好指针的指向。

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
    ListNode* insertionSortList(ListNode* head) {
        ListNode* dummy = new ListNode(-1);
        ListNode* current = dummy;
        
        while (head) {
            ListNode *t = head->next;
            current = dummy;
            while (current->next && current->next->val <= head->val) {
                current = current->next;
            }
            head->next = current->next;
            current->next = head;
            head = t;
        }
        return dummy->next;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本身的coding难度不大，但我认为更重要的意义在于，通过对比在数组和链表中进行选择排序，我们可以知道两种数据结构的特点。链表便于插入的特点在选择排序中体现的非常明显。这道题目的分享到这里，谢谢！
