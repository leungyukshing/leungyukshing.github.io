---
title: LeetCode解题报告（466）-- 671. Second Minimum Node In a Binary Tree
mathjax: true
date: 2021-12-27 23:11:06
---

## Problem

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/15/smbt1.jpg)

```
Input: root = [2,2,5,null,null,5,7]
Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/10/15/smbt2.jpg)

```
Input: root = [2,2,2]
Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 25]`.
- `1 <= Node.val <= 231 - 1`
- `root.val == min(root.left.val, root.right.val)` for each internal node of the tree.

---

## Analysis

&emsp;&emsp;题目给出一颗二叉树，要求找到整棵树中第二小的元素。因为是easy题，所以可以直接对整棵树做一次搜索，把所有的元素都存到一个`unordered_set`中，最后在这个集合中找到第二小的元素。

&emsp;&emsp;但其实这里还有一种稍微好一点的方法，虽然复杂度上并没有不同，但是空间复杂度是function stack的复杂度，因此理论上会更快。题目虽然给的是普通的二叉树，但是也有一些特点，根节点是整颗子树中最小的元素，所以整棵树最小的元素就是根节点`root->val`，记录为`minVal`。紧接着我们可以做一个前序遍历，如果当前节点的值比`minValue`大，说明它有可能是第二小的，于是和`result`比一下，取较小值，之后就不需要再递归下去了，因为这个以这个节点为根的子树的值只会比当前节点的值大，不可能是第二小的数；如果当前节点的值等于`minValue`，说明以当前节点为根的子树中可能存在第二小的数，于是递归左右子树。

## Solution

&emsp;&emsp;这道题目还要处理答案不存在的情况，我们把`result`初始化为`LONG_MAX`，因为节点的值可能到达`INT_MAX`。

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
    int findSecondMinimumValue(TreeNode* root) {
        if (!root) {
            return -1;
        }
        minVal = root->val;
        result = LONG_MAX;
        helper(root);
        return result < LONG_MAX ? result: -1;
    }
private:
    int minVal;
    long result;
    
    void helper(TreeNode* root) {
        if (!root) {
            return;
        }
        
        if (root->val > minVal && root->val < result) {
            result = root->val;
        } else if (root->val == minVal) {
            helper(root->left);
            helper(root->right);
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树中比较有意思的变种，虽然它给的不是BST，但是它的一些特性还是能够帮助我们更快地解题。这道题目的分享到这里，感谢你的支持！
