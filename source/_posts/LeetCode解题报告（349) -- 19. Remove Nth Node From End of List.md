---
title: LeetCode解题报告（349) -- 19. Remove Nth Node From End of List
tags:
  - LeetCode
mathjax: true
date: 2021-04-23 01:17:23

---

## Problem

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Follow up:** Could you do this in one pass?

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

------

## Analysis

&emsp;&emsp;题目给出一个链表，要求删除掉倒数第`n`个节点。同时Follow up要问能不能通过一次遍历解决。先来看简单的版本，我们可以先遍历一遍拿到总长度，然后计算出倒数第几个删掉，然后再遍历一遍到要删除的节点，删掉即可。这样下来要两次遍历，还没能够满足题目的最优解。

&emsp;&emsp;我们思考一下倒数的意思是什么？是从最后开始往前数`n`个，那如果我用两个指针去遍历呢？这两个指针相隔`n`，当最后一个指针指向的是末尾的时候，第一个指针指向的就是倒数第`n`个元素了，这样一次遍历就可以解决。

------

## Solution

&emsp;&emsp;这里有个细节需要注意，我们要删除某个节点实际上是要找到他的前一个节点。假设`p1`指向要删除的节点的前一个节点，`p2`指向的是末尾的前一个节点，两者相距`n`。当`p2->next`为空是，`p1`就到了正确的位置。为了统一代码，我们还是增加一个dummy节点放在头部，这样免去了边界条件的判断。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        
        ListNode* p = dummy, *delay = dummy;
        while (n--) {
            p = p->next;
        }
        
        while (p->next) {
            p = p->next;
            delay = delay->next;
        }
        
        // delete delay->next
        ListNode* temp = delay->next;
        delay->next = temp->next;
        temp->next = NULL;
        delete temp;
        return dummy->next;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较简单的链表题，总的来说链表题无论怎么变花样，双指针是很经典也很常用的方法，可以是两个指针一起走，也可以还是快慢指针。这道题目的分享到这里，感谢你的支持！
