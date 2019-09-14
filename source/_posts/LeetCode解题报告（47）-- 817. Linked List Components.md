---
title: LeetCode解题报告（47）-- 817. Linked List Components.md
tags:
  - LeetCode
date: 2019-09-14 18:58:23

---

## Problem

We are given `head`, the head node of a linked list containing **unique integer values**.

We are also given the list `G`, a subset of the values in the linked list.

Return the number of connected components in `G`, where two values are connected if they appear consecutively in the linked list.

<!-- more -->

**Example 1:**

```
Input:
head: 0->1->2->3
G = [0, 1, 3]
Output: 2
Explanation: 
0 and 1 are connected, so [0, 1] and [3] are the two connected components.
```

**Example 2**

```
Input: 
head: 0->1->2->3->4
G = [0, 3, 1, 4]
Output: 2
Explanation: 
0 and 1 are connected, 3 and 4 are connected, so [0, 1] and [3, 4] are the two connected components.
```



**Note:**

1. If `N` is the length of the linked list given by `head`, `1 <= N <= 10000`.
2. The value of each node in the linked list will be in the range` [0, N - 1]`.
3. `G` is a subset of all values in the linked list.

------

## Analysis

&emsp;&emsp;题目要求我们对一个链表做连通分量数量的计算，连通分量关系在一个数组中给出。即在这个数组中的数组都是相互连通的。对于链表而言，常见的做法就是从头到尾地遍历，最好是便利一次就能把题目做完。由于数组题目是以vector的形式给出，所以我们可以充分利用STL提供的`find()`函数来进行查找操作。

&emsp;&emsp;查找关系的方法已经有了，那么我们只需要提供计算的方式。简单起见，我用了一个bool变量来表示当前是处于一个新的连通分量(false)还是一个正在计算的连通分量(true)。当该变量从true变为false，说明上一个连通分量计算完毕，计数+1；当变量从false变为true，则说明我们开始匹配到数字，新的连通分量开始计算。

------

## Solution

&emsp;&emsp;利用`find()` 在vector中查找每一个链表元素，然后通过布尔值进行计数。注意当最后一个完成时，如果布尔值是true，记得计数+1，因为连通分量正在计算中。

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
    int numComponents(ListNode* head, vector<int>& G) {
        ListNode* p = head;
        int result = 0;
        bool flag = false;
        while (p != NULL) {
            vector<int>::iterator it = find(G.begin(), G.end(), p->val);
            if (!flag) {
                if (it != G.end()) {
                    flag = true;
                }
            }
            else {
                if (it == G.end()) {
                    flag = false;
                    result++;
                }
            }
            p = p->next;
        }
        if (flag) {
            result++;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;一道链表与vector结合的题目，其本质是对数组使用二分查找，但是如果足够熟悉STL的话，使用`find()`能够很快速地解决这题。使用一个布尔变量来进行模式切换也是算法题中很常见的做法，希望这篇博客能够帮助到你。欢迎大家转发、评论，谢谢！
