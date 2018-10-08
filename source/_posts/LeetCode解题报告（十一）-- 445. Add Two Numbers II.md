---
title: LeetCode解题报告（十一）-- 445. Add Two Numbers II
tags:
  - LeetCode
abbrlink: 63155
date: 2018-09-24 11:26:05
---
## Problem
You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up**:
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.
<!-- more -->

**Example:**
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

## Analysis
&emsp;&emsp;这道题是[Add Two Numbers](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%8D%81%EF%BC%89--%202.%20Add%20Two%20Numbers.html)的进阶版，不同的地方在于链表存储数字的顺序改变了，是从高位到低位的。这对于我们逐位做加法是不利的。在**Follow up**中提到了是否可以在不reverse输入的list的前提下解决这个问题，因此这里不讨论reverse list的做法。
&emsp;&emsp;之前提到过，在需要reverse数据的时候，栈是一种很好的数据结构，基于其FIFO的特性，能起到一个reverse的作用。经过栈后，这样题目又变成了我们之前做过的大数加法，瞬间变得非常简单。

## Solution
&emsp;&emsp;我们分别将两个链表的元素`push`入两个栈中，在分别从两个盏中逐一取出元素相加，考虑到进位`add`即可。
&emsp;&emsp;特别注意，最高位也就是链表头的值可能是0，因此需要去除掉前面多余值为0的节点。

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
            add = 0;
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

**运行时间：**约32ms，超过71.00%的CPP代码。

## Summary
&emsp;&emsp;Add Two Numbers系列的两题主要考点是链表的遍历，在逻辑方面考察了大数加法的实现。其中第二题用到了栈这个特殊的数据结构，用于reverse数据。易错点在于要去除掉高位前面多余的0。在第一题中就特别需要一个`prev`指针来实现，而第二题中直接`head = head->next`即可。本题的分析到这里，谢谢您的支持！
