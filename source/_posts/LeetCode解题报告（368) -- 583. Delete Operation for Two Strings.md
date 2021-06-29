---
title: LeetCode解题报告（368) -- 583. Delete Operation for Two Strings
tags:
  - LeetCode
mathjax: true
abbrlink: 41774
date: 2021-05-11 20:22:26
---

## Problem

Given two strings `word1` and `word2`, return *the minimum number of **steps** required to make* `word1` *and* `word2` *the same*.

In one **step**, you can delete exactly one character in either string.

<!-- more -->

**Example 1:**

```
Input: word1 = "sea", word2 = "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```

**Example 2:**

```
Input: word1 = "leetcode", word2 = "etco"
Output: 4
```



**Constraints:**

- `1 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of only lowercase English letters.

------

## Analysis

&emsp;&emsp;题目给出两个字符串，问通过多少个删除操作能够使得两个字符串相等。其实它就是最长公共子序列反着问，只需要计算出最长公共子序列的长度，就能知道需要多少个删除操作。

&emsp;&emsp;为什么呢？思考一下如果两个字符串没有相同的字符，假设分别是`abc`和`def`，此时你需要把它们都删干净才行，所以是两者的长度相加。而如果它们有一个字符相同，如`abc`和`aef`，则需要删除的数量是4。所以计算的方法是`两者长度之和 - 2 * 最长公共子序列长度`。

&emsp;&emsp;最长公共子序列的长度计算就是简单的dp，这里不详细讨论。

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

&emsp;&emsp;这道题目本质上就是最长公共子序列，思路就是dp，然后用这个结果再去计算出删除操作的次数。这道题目的分享到这里，感谢你的支持！
