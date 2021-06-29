---
title: LeetCode解题报告（60）-- 1019. Next Greater Node In Linked List
tags:
  - LeetCode
abbrlink: 1018
date: 2019-09-26 12:31:14
---

## Problem

We are given a linked list with `head` as the first node.  Let's number the nodes in the list: `node_1, node_2, node_3, ...` etc.

Each node may have a *next larger* **value**: for `node_i`, `next_larger(node_i)` is the `node_j.val` such that `j > i`, `node_j.val > node_i.val`, and `j` is the smallest possible choice.  If such a `j` does not exist, the next larger value is `0`.

Return an array of integers `answer`, where `answer[i] = next_larger(node_{i+1})`.

Note that in the example **inputs** (not outputs) below, arrays such as `[2,1,5]` represent the serialization of a linked list with a head node value of 2, second node value of 1, and third node value of 5.

<!-- more -->

**Example 1:**

```
Input: [2,1,5]
Output: [5,5,0]
```

**Example 2:**

```
Input: [2,7,4,3,5]
Output: [7,0,5,5,0]
```

**Example 3:**

```
Input: [1,7,5,1,9,2,5,1]
Output: [7,9,9,9,0,5,0,0]
```

**Note:**

1. `1 <= node.val <= 10^9` for each node in the linked list.
2. The given list has length in the range `[0, 10000]`.

------

## Analysis

&emsp;&emsp;在leetcode上这道题是属于linkedlist类别的，但是这道题目我采取了数组的形式来做。可以借助这道题目来提供一个常用的解题思路。这道题目要求我们求得每一个元素之后第一个比他大的元素，最直接的做法就是两层循环，当然这种低效的方法我们是不考虑的。我们可以先看题目的特点，因为返回的结果与原有的链表的位置是对应下标的，所以我们可以考虑从下标入手。

&emsp;&emsp;从题目给出的几个例子可以看出，对于某一小段序列，如果他是递增的，那么每一个元素下一个比他大的元素就是紧跟着的那个（如[1, 2, 3, 4, 5]，结果就是[2, 3, 4, 5, 0]），而如果是递减的，那么下一个比他大的元素就是这个递减序列结束的那个元素（如example3中的[7, 5, 1, 9]这个序列，前三个元素构成递减，那么这三个元素的下一个比它大的元素都是9）。也就是说我们需要记录某一小段递减的元素，对于递增序列而言，则不需要记录，这种规律是可以用栈来实现的（栈加上栈顶的大小比较是可以维护一个有序的序列的）。

------

## Solution

&emsp;&emsp;具体的做法是，首先将链表转化为数组，这样我们就可以通过下标来操作。然后遍历这个数组，每次做如下的判断：

- 如果该元素`arr[i]` >　`arr[top]`，则说明是递增的，所以`top`位置填的就是`arr[i]`;
- 否则，则说明是递减的，所以需要入栈

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
    vector<int> nextLargerNodes(ListNode* head) {
        // list -> array
        ListNode* p1 = head;
        vector<int> arr;
        while (p1 != NULL) {
            arr.push_back(p1->val);
            p1 = p1->next;
        }
        
        int size = arr.size();
        vector<int> result(size);
        stack<int> st;
        for (int i = 0; i < size; i++) {
            while (!st.empty() && arr[st.top()] < arr[i]) {
                result[st.top()] = arr[i];
                st.pop();
            }
            st.push(i);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道和链表、数组有关的题目，在解决数组方面的问题时，多重的循环通常是能够解题，但是必定有更快的方法。解决数组题目有一点很重要的就是把握好元素之间的大小关系，然后可以充分利用下标和其他数据结构（如queue、stack、map）来配合解决问题。这道题的分享到这里，欢迎大家评论、转发，感谢您的支持！
