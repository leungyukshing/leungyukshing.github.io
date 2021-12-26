---
title: LeetCode解题报告（453）-- 501. Find Mode in Binary Search Tree
mathjax: true
date: 2021-12-25 22:16:19
---

## Problem

Given the `root` of a binary search tree (BST) with duplicates, return *all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (i.e., the most frequently occurred element) in it*.

If the tree has more than one mode, return them in **any order**.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
- The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
- Both the left and right subtrees must also be binary search trees.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg)

```
Input: root = [1,null,2,2]
Output: [2]
```

**Example 2:**

```
Input: root = [0]
Output: [0]
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-105 <= Node.val <= 105`

 

**Follow up:** Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

---

## Analysis

&emsp;&emsp;题目给出一颗BST，要求找出出现最多的节点值，如果有多个则返回全部。对于一颗普通的二叉树来说，很直接的做法就是遍历一遍，把所有的值都用一个map存起来，最后找到出现次数最多的值即可。但这里我们有的是BST，应该利用好这个性质，而且题目给了Follow up是不使用额外的空间，所以也不需要用map来统计。

&emsp;&emsp;BST最大的特点就是中序遍历输出有序序列，而相同的元素肯定是靠在一起的，我们就可以利用这个性质去解题。我们全局维护一个`pre`表示前一个节点的值，当我们进行中序遍历是，我们对比当前的值和`pre`，如果一样则说明是相同的数字，`count`自增，如果不一样则把`count`重置为1。然后再记录一个最大的freq，每次处理当前节点后维护一下这个maxFreq即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
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
    vector<int> findMode(TreeNode* root) {
        vector<int> result;
        int pre = INT_MIN, count = 1, maxFreq = 0;
        helper(root, pre, count, maxFreq, result);
        return result;
    }
private:
    void helper(TreeNode* root, int &pre, int &count, int &maxFreq, vector<int>& result) {
        if (!root) {
            return;
        }
        
        helper(root->left, pre, count, maxFreq, result);
        if (pre != INT_MIN) {
            count = (root->val == pre? count + 1: 1);
        }
        
        if (count > maxFreq) {
            result.clear();
            result.push_back(root->val);
            maxFreq = count;
        } else if (count == maxFreq) {
            result.push_back(root->val);
        }
        pre = root->val;
        helper(root->right, pre, count, maxFreq, result);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道很典型地利用BST中序遍历结果是有序的这个特点求解的题目，一般来说，这种题目都可以用一个值记录前一个节点的信息，然后与当前节点进行比较。这道题目的分享到这里，感谢你的支持！
