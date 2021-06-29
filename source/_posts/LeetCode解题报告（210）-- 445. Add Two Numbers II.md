---
title: LeetCode解题报告（210）-- 445. Add Two Numbers II
tags:
  - LeetCode
mathjax: true
abbrlink: 35789
date: 2020-11-08 02:20:41
---

## Problem

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

<!-- more -->

**Example:**

```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

------

## Analysis

&emsp;&emsp;这是一道基于链表的大数加法，但是增加了一点难度。首先是链表给出的顺序是从高位到低位的，而加法需要从地位到高位。为了解决这个问题我们可以把链表先翻转过来，这样是比较好操作的。但是题目的Follow up提到说能不能不修改原来的数据结构，这样就只能借助额外的空间了。

&emsp;&emsp;顺序要反转我们常用的数据结构当然是栈了，我们只需要分别把两个链表都放到栈中，然后从栈顶开始处理，最后就能算出结果了。

------

## Solution

&emsp;&emsp;这道题目的coding有挑战的地方是如何去构建新的链表，因为牵涉到进位。我这里的做法就是默认都有进位，所有的完成之后再对head进行判断，如果最高位相加没有产生进位，那么就返回`head->next`。这样处理的好处就是前面的代码可以统一。

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;
        while (l1) {
            s1.push(l1->val);
            l1 = l1->next;
        }
        while (l2) {
            s2.push(l2->val);
            l2 = l2->next;
        }
        
        ListNode* result = new ListNode(0);
        int sum = 0;
        while (!s1.empty() || !s2.empty()) {
            if (!s1.empty()) {
                sum += s1.top();
                s1.pop();
            }
            if (!s2.empty()) {
                sum += s2.top();
                s2.pop();
            }
            ListNode* head = new ListNode(sum / 10);
            result->val = sum % 10;
            head->next = result;
            result = head;
            sum /= 10;
        }
        return result->val == 0 ? result->next: result;
    }
};
```

------

## Summary

&emsp;&emsp;大数加法的题目在不同数据结构上我们都做过，这次是不同顺序。显然在解决这类问题的时候我们要借助一些特殊的数据结构去帮助我们。这道题目的分享到这里，谢谢！
