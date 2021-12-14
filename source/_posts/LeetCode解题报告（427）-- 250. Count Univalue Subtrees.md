---
title: LeetCode解题报告（427）-- 250. Count Univalue Subtrees
mathjax: true
date: 2021-12-14 16:20:13
---

## Problem

Given the `root` of a binary tree, return the number of **uni-value** subtrees.

A **uni-value subtree** means all nodes of the subtree have the same value.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/08/21/unival_e1.jpg)

```
Input: root = [5,1,5,5,5,null,5]
Output: 4
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [5,5,5,5,5,null,5]
Output: 6
```

**Constraints:**

- The numbrt of the node in the tree will be in the range `[0, 1000]`.
- `-1000 <= Node.val <= 1000`

------

## Analysis

&emsp;&emsp;题目要求找到值都是一样的子树的数量。对于二叉树的题目，优先考虑递归解决。首先每个叶子节点都符合要求，因为一个节点本身就是一颗子树，然后再想怎么去把信息传递给父节点。父节点可以对左子树及右子树递归调用，判断是否值都是一样，如果两边的值都一样并且和父节点本身的值也一样，这样向上返回true，否则就是false。然后我们再用一个全局的变量统计数量即可。

## Solution

&emsp;&emsp;无

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
    int countUnivalSubtrees(TreeNode* root) {
        if (!root) {
            return 0;
        }
        result = 0;
        helper(root);
        return result;
    }
private:
    int result;
    bool helper(TreeNode* root) {
        
        if (!root->left && !root->right) {
            result++;
            return true;
        }
        
        bool flag = true;
        // cout << root->val << endl;
        if (root->left) {
            flag = helper(root->left) && root->val == root->left->val;
        }
        
        if (root->right) {
            flag = helper(root->right) && flag && root->val == root->right->val;
        }
        
        if (!flag) {
            return false;
        }
        result++;
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;二叉树的题目基本都是一个套路，关键要搞明白子节点要向父节点传递什么信息。对于统计子树类的题目，一般是递归函数返回一个结果用于传递信息（比如子树是否满足要求、子树的和、差），然后再用一个全局变量去统计符合要求的数量。这道题目的分享到这里，感谢你的支持！
