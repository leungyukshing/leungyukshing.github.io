---
title: LeetCode解题报告（200）-- 142. Linked List Cycle II
tags:
  - LeetCode
mathjax: true
date: 2020-10-27 20:54:11

---

## Problem

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that pos is not passed as a parameter**.

**Notice** that you **should not modify** the linked list.

**Follow up:**

Can you solve it using `O(1)` (i.e. constant) memory?

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

------

## Analysis

&emsp;&emsp;链表回环的系列题，在前面的题目我们已经知道通过**快慢指针**的方法来判断链表中是否存在环。这道题目在这个基础上，还要加上定位这个环的起点。这个难度一下子就加大了，一开始我也没有特别好的思路，还是先用快慢指针先找到他们相遇的位置。

&emsp;&emsp;以Example1为例，快慢指针是在-4相遇，然后我们分别看一下两个指针走过的路：

- 慢指针：3 -> 2 -> 0 -> -4（head->环的起点->相遇点）
- 快指针：3 -> 0 -> 2 -> -4（head->环的起点->相遇点->环的起点->相遇点）

&emsp;&emsp;快指针走的路程是慢指针的两倍，同时如果存在环的话，快指针肯定也比慢指针多走了一个环（否则的话不会相遇）。所以根据上面对两个指针的路程分析，可以得出：`head->环的起点->相遇点 = 相遇点->环的起点->相遇点`。抵消掉环的起点->相遇点的部分，就得出`head->环的起点 = 相遇点->环的起点`。到这个时候，因为快慢指针都处于相遇点，所以只需要把一个指针指向head，然后两个指针都是一步一走，到他们再次相遇的时候，就是环的起点了。

------

## Solution

&emsp;&emsp;注意不存在环的处理，以及循环过程中可能出现的空指针问题。

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
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                break;
            }
        }
        if (fast == NULL || fast->next == NULL) {
            return NULL;
        }
        
        slow = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是很典型的快慢指针处理路程的题目。在使用快慢指针的方法的时候，一定要掌握好他的原理和特点，快的比慢的多走了一倍路程，再通过对路程的分析找到规律。这道题目的分享到这里，谢谢！
