---
title: LeetCode解题报告（31）-- 24. Swap Nodes in Pairs
tags:
  - LeetCode
abbrlink: 36300
date: 2018-11-12 07:31:49
---
## Problem
Given a linked list, swap every two adjacent nodes and return its head.

**Example**:
```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```
<!-- more -->

**Note:**
  + Your algorithm should use only constant extra space.
  + You may not modify the values in the list's nodes, only nodes itself may be changed.

## Analysis
&emsp;&emsp;这道题考察的是关于链表的操作，难点在于它要求交换相邻的两个节点。同时题目要求不能更改节点的值，我们只能通过更改节点之间的指针来实现。
&esmp;&emsp;因为是交换相邻的两个节点，因此我们需要使用两个指针分别指向先后两个节点。如果链表的节点数是偶数，那么每两个交换；如果链表的节点数是奇数，那么最后一个节点则不需要操作。如果给出两个节点，要求交换，这个操作并不难，`first->next = second->next; second->next = first`即可实现。难度在于如果切换至下一对节点。首先我们需要判断后面是否有节点，这里对`first`判断是否为null即可。循环的退出条件是只剩最后一个节点或后面没有节点。
&emsp;&emsp;运行后发现仍然报错了，出错的原因是，上一对的`first`应该指向的是当前对的`second`。如果需要在上一对处理的时候判断，那么将会增加大量的代码。这里我将判断放到了当前对中，因此只需要额外增加一个变量`last`来表示上一对的`first`。在当前对中，直接将`last->next = second`来完成两对之间的连接。这个办法显然快很多。
&emsp;&emsp;至此，整个算法思路就很明确了。同时要注意两个问题：
  1. 当链表长度小于等于2的时候，直接返回。
  2. 我们需要用一个变量`result`来返回交换后的表头，因为交换后的表头一定是第一对中的`second`，所以可以直接赋值`result = second`。

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
    ListNode* swapPairs(ListNode* head) {
        ListNode *first;
        ListNode *second;
        if (head == NULL || head->next == NULL) {
            return head;
        }

        first = head;
        second = head->next;
        ListNode* result = second;
        ListNode *last = first;

        while (second != NULL) {
            last->next = second;
            first->next = second->next;
            second->next = first;
            last = first;
            first = first->next;
            if (first == NULL) {
                break;
            }
            second = first->next;
        }

        return result;
    }
};
```
**运行时间：**约0ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;这道题目考点还是链表的操作，相比起以前的插入删除，这道题的难度有所提高，做起来还是花了点时间。我的建议是关于链表的题目多在纸上画，模拟一下算法的过程，然后可以在程序中打印一下相关的值，同时牵涉到指针问题需要特别注意指针为空的情况。本题的分析就到这里，感谢您的支持！
