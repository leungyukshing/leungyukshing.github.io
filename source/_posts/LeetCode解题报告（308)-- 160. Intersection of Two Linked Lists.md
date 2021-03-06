---
title: LeetCode解题报告（308)-- 160. Intersection of Two Linked Lists
tags:
  - LeetCode
mathjax: true
date: 2021-03-06 15:28:02

---

## Problem

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

![Example](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)

begin to intersect at node c1.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/06/29/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/06/29/160_example_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: null
Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

**Notes:**

- If the two linked lists have no intersection at all, return `null`.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Each value on each linked list is in the range `[1, 10^9]`.
- Your code should preferably run in O(n) time and use only O(1) memory.

------

## Analysis

&emsp;&emsp;这道题目给出了两个链表，要求找到两个链表的交点，特别注意这个交点指的是节点的地址相同，而不是节点的值相同。暴力的解法是对比两条链表上每一个节点的地址，如果找到相同的，就说明这个是交点。但是题目提醒可以使用$O(n)$的时间复杂度，这里就有优化的空间了。

&emsp;&emsp;这里我们因为要找的是交点，而难的地方在于两个链表长度可能不一致。因为交点后面的长度肯定是一样的，所以问题就是交点前的长度不一致问题。因此我们就需要把交点前的长度统一到一致。我们可以先分别计算出两个链表的长度，然后取两者之差，就能得到较长的链表`A`比较短的链表`B`长了多少，那么先从较长的链表`A`开始，走这段距离，然后这个时候链表`B`也一起开始走，那么它们的长度就是一致的，这样往后搜索知道找到地址相同的节点，就说明找到了交点。

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int sizeA = getSize(headA), sizeB = getSize(headB);
        
        if (sizeA > sizeB) {
            for (int i = 0; i < sizeA - sizeB; i++) {
                headA = headA->next;
            }
        } else {
            for (int i = 0; i < sizeB - sizeA; i++) {
                headB = headB->next;
            }
        }
        
        while (headA && headB && headA != headB) {
            headA = headA->next;
            headB = headB->next;
        }
        
        return (headA && headB) ? headA: NULL;
    }
private:
    int getSize(ListNode *head) {
        int size = 0;
        while (head) {
            size++;
            head = head->next;
        }
        return size;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是链表题目中比较新颖的一道，关键是把握住要判断地址是否相同来确认交点。这道题目的分享到这里，感谢你的支持！
