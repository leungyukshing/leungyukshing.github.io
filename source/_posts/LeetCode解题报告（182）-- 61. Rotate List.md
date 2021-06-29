---
title: LeetCode解题报告（182）-- 61. Rotate List
tags:
  - LeetCode
mathjax: true
abbrlink: 49119
date: 2020-10-07 22:10:08
---

## Problem

Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

<!-- more -->

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

------

## Analysis

&emsp;&emsp;很久没有碰过链表类型的题目了，这道题目是旋转链表，最关键的地方就是找到旋转后的`head`。题目给出了旋转的位置数`k`，因为旋转是从最后一个开始旋转的，所以实际上`head`移动的位置是`size - k`。我们需要两次遍历，第一次遍历整个链表得到总长度`size`，然后计算出`head`的位置。把链表首尾相连，再进行第二次遍历，找到`head`所在的位置，把`head`前面的指针断掉，这样就形成了旋转后的链表了。

------

## Solution

&emsp;&emsp;因为旋转的位置数可能比整个链表的长度要大，所以计算`head`新位置的公式为`pos = size - k % size`。找到新的`head`之后，记得先记录下`newHead`的位置，再把前一个node的指针置空。

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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) {
            return NULL;
        }
        int size = 1;
        ListNode* temp = head;
        while (temp->next) {
            temp = temp->next;
            size++;
        }
        temp->next = head;
        
        int pos = size - k % size;
        // temp = head;
        for (int i = 0; i < pos; i++) {
            temp = temp->next;
        }
        ListNode* newHead = temp->next;
        temp->next = NULL;
        return newHead;
    }
};
```

------

## Summary

&emsp;&emsp;解决链表类的题目最关键的就是抓住`head`的位置，这道题目的巧妙之处在于直接通过`k`来计算出新`head`的位置，在coding方面没有太多的难点。这道题目的分享到这里，谢谢！
