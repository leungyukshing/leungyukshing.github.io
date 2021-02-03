---
title: LeetCode解题报告（277）-- 23. Merge k Sorted Lists
tags:
  - LeetCode
mathjax: true
date: 2021-02-03 21:42:04

---

## Problem

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

<!-- more -->

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

**Constraints:**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` won't exceed `10^4`.

------

## Analysis

&emsp;&emsp;这道题目要求合并`k`个有序链表，其实跟合并两个有序链表是一样的。合并两个的话，只需要对比A和B的元素，然而合并`k`个的话，就需要对比`k`个队列的头。这里我们就要善用数据结构了。我们的需求是在`k`个元素中维护一个最小值，每次都取出最小的那个，那符合这个用途的无疑就是最小堆（优先队列）了。

&emsp;&emsp;确定好使用优先队列，这道题目就变得很容易。把每个队列的头放入到优先队列中，每次从中取出头部（值最小的），然后把它的后一个节点也加入到队里中，一直遍历直到队列为空。

------

## Solution

&emsp;&emsp;因为拿出一个元素后，需要把它后面一个元素放入到队列中，所以队列存的不是数值，而是节点的指针。

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode* &a, ListNode* &b) {
            return a->val > b->val;
        };
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> q(cmp);
        int size = lists.size();
        for (int i = 0; i < size; i++) {
            if (lists[i]) {
                q.push(lists[i]); 
            }
        }
        ListNode* dummy = new ListNode(-1), *p = dummy;
        while (!q.empty()) {
            ListNode* tmp = q.top();
            q.pop();
            p->next = tmp;
            p = p->next;
            if (tmp->next) {
                q.push(tmp->next);
            }
        }
        return dummy->next;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是优先队列的一个经典应用，看似很复杂的题目，只要使用合适的数据结构，就能够很轻易地解决。这道题这道题目的分享到这里，谢谢您的支持！
