---
title: LeetCode解题报告（62）-- 328. Odd Even Linked List
tags:
  - LeetCode
date: 2019-09-27 17:43:10

---

## Problem

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

<!-- more -->

**Example 1:**

```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL
```

**Example 2:**

```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```

**Example 3:**

```
Input: [1,7,5,1,9,2,5,1]
Output: [7,9,9,9,0,5,0,0]
```

**Note:**

- The relative order inside both the even and odd groups should remain as it was in the input.
- The first node is considered odd, the second node even and so on ...

------

## Analysis

&emsp;&emsp;这道题目的意思很简单，给定一个链表，要求我们把它分为两个链表：一个是奇数位的节点组成的链表，另一个是偶数位节点组成的链表，最后再将他们合在一起返回。

　&emsp;&emsp;大致的思路是，对链表进行一次遍历，然后用两个头、两个游标分别将链表的每一个节点分配给不同的子链表，最后将偶数位链表的头拼接到奇数位链表的尾即可。

------

## Solution

&emsp;&emsp;使用一个指针遍历主链表，每个子链表分别用两个指针：头指针和遍历指针。另外增加一个bool变量来判断是奇数位还是偶数位。**注意，偶数位链表的最后一个节点记得设置next为NULL**，否则很容易形成环状链表而导致TLE。

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
    ListNode* oddEvenList(ListNode* head) {
        ListNode* odd_head;
        ListNode* even_head;
        if (head == NULL) {
            return NULL;
        }
        odd_head = head;
        even_head = head->next;
        if (even_head == NULL) {
            return odd_head;
        }
        ListNode* p = even_head->next;
        ListNode* p1 = odd_head;
        ListNode* p2 = even_head;
        bool flag = false;
        while (p != NULL) {
            if (flag) {
                p2->next = p;
                p2 = p2->next;
            }
            else {
                p1->next = p;
                p1 = p1->next;
            }
            flag = !flag;
            p = p->next;
        }
        p2->next = NULL;
        p1->next = even_head;
        return odd_head;
    }
};
```

------

## Summary

&emsp;&emsp;这是切分链表的题目，题目的要求还算比较简单，这类题目主要是细心处理好**空指针问题**和**环状问题**就可以了。这道题的分享到这里，欢迎大家评论、转发，最后，谢谢您的支持！
