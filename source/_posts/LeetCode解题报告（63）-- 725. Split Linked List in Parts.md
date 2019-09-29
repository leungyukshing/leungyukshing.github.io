---
title: LeetCode解题报告（63）-- 725. Split Linked List in Parts
tags:
  - LeetCode
date: 2019-09-29 12:33:11

---

## Problem

Given a (singly) linked list with head node `root`, write a function to split the linked list into `k` consecutive linked list "parts".

The length of each part should be as equal as possible: no two parts should have a size differing by more than 1. This may lead to some parts being null.

The parts should be in order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal parts occurring later.

Return a List of ListNode's representing the linked list parts that are formed.

Examples 1->2->3->4, k = 5 // 5 equal parts [ [1], [2], [3], [4], null ]

<!-- more -->

**Example 1:**

```
Input: 
root = [1, 2, 3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The input and each element of the output are ListNodes, not arrays.
For example, the input root has root.val = 1, root.next.val = 2, \root.next.next.val = 3, and root.next.next.next = null.
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but it's string representation as a ListNode is [].
```

**Example 2:**

```
Input: 
root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
Output: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.
```

**Note:**

- The length of `root` will be in the range `[0, 1000]`.

- Each value of a node in the input will be an integer in the range `[0, 999]`.

- `k` will be an integer in the range `[1, 50]`.

------

## Analysis

&emsp;&emsp;这道题是链表题目中有关指针操作比较复杂的一道题，题目给出一个链表，要求我们将链表划分成连续的`k`份（即每一份里面都是连续的）。并且不同份之间的节点数量相差不能大于1，换句话说，**在尽可能均分的前提下，将多余的节点靠前分配**。从这里我们把分配的问题看成是这样一个流程：先均分成k份，在剩余的里面，从头到后再分给每一份。

&emsp;&emsp;这样我们就得到了两个问题：一是，均分之后每一份有多少；二是，前面有多少份是需要多加的。第一个问题很简单，均分就直接使用整数除法就能得到。第二个问题在这个题目的限定下也很简单，因为每一份最多只能加1，所以对`k`取余的结果就是需要+1的份数。

&emsp;&emsp;解决上面两个问题之后，我们遍历链表，计算好份数和一份中的节点的数量就可以了。

------

## Solution

&emsp;&emsp;我们首先遍历一边链表获取总长度，然后分别使用整数除法和取余获得我们需要的两个信息，然后遍历链表即可。中间的操作都是一些简单的判断，需要注意，**每一个小份都有一个头指针和一个游标，游标每次都需要设置next为NULL，不然会直接连接到下一份**。

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
    vector<ListNode*> splitListToParts(ListNode* root, int k) {
        vector<ListNode*> result;
        
        ListNode* p = root;
        int size = 0;
        while (p != NULL) {
            size++;
            p = p->next;
        }
        
        int basic_num = size / k;
        int added_num = size % k;
        
        p = root;
        int group = 0;
        for (int i = 0; i < k; i++) {
            ListNode* head = p;
            ListNode* p1 = head;
            if (p == NULL) {
                result.push_back(p);
                continue;
            }
            p = p->next;
            p1->next = NULL;
            int limit = basic_num - 1;
            if (i < added_num) {
                limit++;
            }
            int idx = 0;
            while (p != NULL && idx < limit) {
                p1->next = p;
                p1 = p1->next;
                p = p->next;
                p1->next = NULL;
                idx++;
            }
            result.push_back(head);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是链表切分中比较复杂的题目，由于题目的限制，实际上这道题的做法就变得简单。易错点是小份的最后一个节点很容易忘记设置next为NULL而导致连接到下一个节点。这道题的分享到这里，欢迎大家评论、转发，最后，谢谢您的支持！
