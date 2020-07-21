---
title: LeetCode解题报告（104）-- 203. Remove Linked List Elements
tags:
  - LeetCode
date: 2020-07-21 22:36:28
mathjax: true


---

## Problem

Remove all elements from a linked list of integers that have value **val**.

<!-- more -->

**Example:**

```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

------

## Analysis

&emsp;&emsp;这道题目是非常经典的链表删除操作。这类题目一般有两种做法：

1. 找到要删除的元素，然后和后一个元素交换，改为删掉后一个元素的内存空间；
2. 使用前后指针，`pre`指针在前，`p`指针在后，`p`指向要被删除的元素，最后删除掉`p`指向的内存区域。

&emsp;&emsp;因为这道题目要删掉的元素可能有多个，而且可能存在于链表的尾部，如果使用第一种方法的话就需要考虑多个边界条件。所以这里使用第二种方法，但是第二种方法也有局限的地方——在第一个节点的时候没有`pre`指针。这里为了代码的统一，我们可以手动在`head`前面加入一个dummy节点。这样整个链表的遍历就统一了。

------

## Solution

&emsp;&emsp;技巧就是在`head`前加入一个dummy节点，`pre` 指向这个节点。

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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *dummy = new ListNode(-1), *pre = dummy;
        dummy->next = head;
        while (pre->next) {
            if (pre->next->val == val) {
                ListNode *t = pre->next;
                pre->next = t->next;
                t->next = NULL;
                delete t;
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

&emsp;&emsp;链表的删除是比较基础的知识点，但是很容易有null point的情况发生，所以在做这类题目的时候要特别注意判断指针是否为空。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
