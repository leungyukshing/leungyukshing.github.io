---
title: LeetCode解题报告（317)-- 1721. Swapping Nodes in a Linked List
tags:
  - LeetCode
mathjax: true
date: 2021-03-15 01:11:51

---

## Problem

You are given the `head` of a linked list, and an integer `k`.

Return *the head of the linked list after **swapping** the values of the* `kth` *node from the beginning and the* `kth` *node from the end (the list is **1-indexed**).*

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
```

**Example 3:**

```
Input: head = [1], k = 1
Output: [1]
```

**Example 4:**

```
Input: head = [1,2], k = 1
Output: [2,1]
```

**Example 5:**

```
Input: head = [1,2,3], k = 2
Output: [1,2,3]
```

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 105`
- `0 <= Node.val <= 100`

------

## Analysis

&emsp;&emsp;题目给出一个链表，要求交换从头开始的第`k`个数和倒数第`k`个数。从链表的特点看，除非遍历一边，否则无法知道整体的长度，因此也无法知道倒数第`k`个数是什么。所以思路有两个：

&emsp;&emsp;第一，先遍历一边链表，获取到链表的总长度，然后第二次再遍历，记录下第`k`个节点和倒数第`k`个节点，然后进行数值的交换即可；

&emsp;&emsp;第二，直接把链表转为数组操作，操作完成后把值重新赋值回去。

&emsp;&emsp;从性能和效率上讲，第一种占用的空间更少，但是操作指针的次数变多，如果是为了解题方便我会选择第二种，直接开一个大的数组去存放。

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
    ListNode* swapNodes(ListNode* head, int k) {
        int arr[100000] = {-1};
        ListNode* p = head;
        int idx = 0;
        while (p) {
            arr[idx++] = p->val; 
            p = p->next;
        }
        // swap
        int temp = arr[k - 1];
        arr[k - 1] = arr[idx - k];
        arr[idx - k] = temp;
        
        // rebuild the linked list
        p = head;
        for (int i = 0; i < idx; i++) {
            p->val = arr[i];
            p = p->next;
        }
        return head;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是链表遍历中较为简单的题目，解决链表类题目要清楚链表的特性，如果要知道链表的长度，或者从后开始选，基本上就要遍历一遍完整的，或者转化为数组操作。这道题目的分享到这里，感谢你的支持！
