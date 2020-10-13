---
title: LeetCode解题报告（187）-- 148. Sort List
tags:
  - LeetCode
mathjax: true
date: 2020-10-13 19:09:22


---

## Problem

Given the `head` of a linked list, return *the list after sorting it in **ascending order***.

**Follow up:** Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

**Example 3:**

```
Input: head = []
Output: []
```

**Constraints:**

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `-105 <= Node.val <= 105`

------

## Analysis

&emsp;&emsp;这道题目是链表的排序，题目给出了时间复杂度的要求，很明显就是归并排序了。归并的思路其实就是分而治之，和数组下的归并是一个道理。链表场景下有几个不同的地方：

1. 怎么样对半分，在数组里面我们可以知道长度，通过下标计算就可以分为前后各一半。
2. 如何合并起来，而且是在不使用额外空间的情况下。

&emsp;&emsp;对于第一点，解决的方案就是**快慢指针**。慢指针一次走一步，快指针一次走两步，所以当快指针走完的时候，慢指针恰好在一半的位置。这个时候把链表从慢指针的地方切开，就能得到前后两半了。

&emsp;&emsp;对于第二点，合并的时候比较两个链表的元素，然后链表的头往后移，一直到最后，这个和数组的合并是基本一致的，最后多出来的放到后面就可以了。

------

## Solution

&emsp;&emsp;在合并的时候为了实现方便，可以用一个dummy节点作为头，最后返回的时候返回`dummy->next`就可以了。

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
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* slow = head, *fast = head, *pre = head;
        
        while (fast && fast->next) {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        pre->next = NULL;
        return merge(sortList(head), sortList(slow));
    }
private:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* cur = dummy;
        
        while(l1 && l2) {
            if (l1->val > l2->val) {
                cur->next = l2;
                l2 = l2->next;
            } else {
                cur->next = l1;
                l1 = l1->next;
            }
            cur = cur->next;
        }
        if (l1) {
            cur->next = l1;
        }
        if (l2) {
            cur->next = l2;
        }
        return dummy->next; 
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是链表结构的归并排序，比较少做。这里有两个总结的点，第一个是使用快慢指针来对链表二分，第二个是在处理链表题目的时候，可以通过dummy节点来优化代码逻辑。这道题目的分享到这里，谢谢！
