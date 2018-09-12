---
title: LeetCode解题报告（一）-- 445. Add Two Numbers II
tags:
  - LeetCode
abbrlink: 61311
date: 2018-09-05 16:50:17
---
## Problem
**Description**: You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example**:
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```
<!-- more -->

## Analysis
&emsp;&emsp;从题目看是一个关于链表的加法运算，其涉及两个知识点：**链表**以及**大数加法**，难点在于题目给出的输入是**从高位到低位的**，而加法则需要**从低位到高位**。

## Solution
&emsp;&emsp;为了解决链表的高位问题（不考虑把链表reverse的方法，效率太低），这里可以巧妙地借助stack来实现。分别将两个链表的数字压入两个栈中。再从栈顶逐个取出相加即可。

Tips:
  + 逐位相加时，需要考虑上一位的进位。
  + 最后返回链表head指针时，需要判断最高位是否为0，若为0，则返回head-next；否则，直接返回head.

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
        stack<int> s1;
        stack<int> s2;

        while (l1 != NULL) {
            s1.push(l1->val);
            l1 = l1->next;
        }
        while (l2 != NULL) {
            s2.push(l2->val);
            l2 = l2->next;
        }

        int add = 0;
        ListNode* result = new ListNode(0);
        while (!s1.empty() || !s2.empty()) {
            int sum = result->val;
            add = 0; // reset
            if (!s1.empty()) {
                sum += s1.top();
                s1.pop();
            }
            if (!s2.empty()){
                sum += s2.top();
                s2.pop();
            }

            if (sum >= 10) {
                sum %= 10;
                add = 1;
            }

            result->val = sum;
            ListNode* newNode = new ListNode(add);
            newNode->next = result;
            result = newNode;
        }
        return result->val == 0 ? result->next : result;
    }
};
```

## Summary
&emsp;&emsp;开始的时候，我的思路是分别计算出两条链所代表的数，然后相加后再逐个数字提取出来插入链表，这种做法的问题在于无法处理数值较大的数。因此就把思路转向了逐位求和的方式。在数据结构中，**stack是一个可以用于reverse数据的工具**，因此我使用了stack来在链表中获取最低位数字，随后的求和就比较简单了。本题的分析就到这里，谢谢！
