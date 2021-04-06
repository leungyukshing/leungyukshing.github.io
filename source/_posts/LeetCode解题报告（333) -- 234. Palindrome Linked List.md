---
title: LeetCode解题报告（333) -- 234. Palindrome Linked List
tags:
  - LeetCode
mathjax: true
date: 2021-04-06 01:31:26

---

## Problem

Given the `head` of a singly linked list, return `true` if it is a palindrome.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
Input: head = [1,2]
Output: false
```

**Constraints:**

- The number of nodes in the list is in the range `[1, 105]`.
- `0 <= Node.val <= 9`

 

**Follow up:** Could you do it in  time and  space?

------

## Analysis

&emsp;&emsp;题目给出一个链表，要求判断是否回文链表。判断回文如果是放在数组中做，就很简单了，因为我们可以通过下标直接访问，所以用头尾指针不断对比即可。但是放到链表中，就不能这么做了。对于链表，我们只能一个方向遍历，但是判断回文显然要从两个方向遍历，为了实现两个方向遍历，在不使用额外空间的情况下，只能把链表翻转。

&emsp;&emsp;有了翻转这个思路，题目就变得简单了，我们只需要从中间把链表翻转，然后对两部分逐一比较就能判断是否回文了。

------

## Solution

&emsp;&emsp;这里我们只需要对链表的一半进行翻转，显然是对后半部分翻转比较容易（不需要考虑很多边界条件）。这里不需要对奇偶数量的节点做特殊处理，因为在对比前后两部分的时候，我们只需要对比两条链表上都有的节点。

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
    bool isPalindrome(ListNode* head) {
        if (!head || !head->next) {
            return true;
        }
        ListNode *fast = head, *slow = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        // reverse[slow->next,end]
        ListNode *head2 = slow->next, *pre = slow->next, *cur = pre->next;
        slow->next = NULL;
        while (cur) {
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        head2->next = NULL;
        
        // check
        ListNode* p1 = head, *p2 = pre;
        while (p2) {
            if (p1->val != p2->val) {
                return false;
            }
            p2 = p2->next;
            p1 = p1->next;
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然是判断回文数，但其本质考察的是部分翻转链表，因此翻转链表这个知识点是非常关键的，包括全部翻转、部分翻转。这道题目的分享到这里，感谢你的支持！
