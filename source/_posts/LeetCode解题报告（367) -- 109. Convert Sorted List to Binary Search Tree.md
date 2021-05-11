---
title: LeetCode解题报告（367) -- 109. Convert Sorted List to Binary Search Tree
tags:
  - LeetCode
mathjax: true
date: 2021-05-11 13:29:30

---

## Problem

Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [0]
Output: [0]
```

**Example 4:**

```
Input: head = [1,3]
Output: [3,1]
```



**Constraints:**

- The number of nodes in `head` is in the range `[0, 2 * 104]`.
- `-10^5 <= Node.val <= 10^5`

------

## Analysis

&emsp;&emsp;题目要求根据一个有序链表去构建一颗BST。我们知道BST中序遍历的结果就是有序的，所以可以通过这个特点反过来构建BST。这里我们先不考虑数据结构，假设用的是数组，怎么构建？

&emsp;&emsp;首先根节点是最中间的元素，然后左手边的元素就是左子树，右手边的元素就是右子树，只需要递归下去构建即可。这样看来，如果用的是数组，直接指定下标即可，但是这里给的是链表，稍微复杂一点。我们每次递归时，都需要找出中间的节点，方法是用快慢指针，找到中间的节点后，左边的元素扔进去左子树递归，节点右边的元素扔进去右子树递归即可。

------

## Solution

&emsp;&emsp;无。

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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return NULL;
        if (!head->next) return new TreeNode(head->val);
        ListNode *slow = head, *fast = head, *last = slow;
        while (fast->next && fast->next->next) {
            last = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        fast = slow->next;
        last->next = NULL;
        TreeNode *cur = new TreeNode(slow->val);
        if (head != slow) cur->left = sortedListToBST(head);
        cur->right = sortedListToBST(fast);
        return cur;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质上还是由数组构建BST的变种，这里换成了链表，整体的思路是不变的，唯一变化的是需要用快慢指针找中间节点。这道题目的分享到这里，感谢你的支持！
