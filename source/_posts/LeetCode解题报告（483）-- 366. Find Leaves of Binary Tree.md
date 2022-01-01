---
title: LeetCode解题报告（483）-- 366. Find Leaves of Binary Tree
mathjax: true
date: 2022-01-01 13:25:47
---

## Problem

Given the `root` of a binary tree, collect a tree's nodes as if you were doing this:

- Collect all the leaf nodes.
- Remove all the leaf nodes.
- Repeat until the tree is empty.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/03/16/remleaves-tree.jpg)

```
Input: root = [1,2,3,4,5]
Output: [[4,5,3],[2],[1]]
Explanation:
[[3,5,4],[2],[1]] and [[3,4,5],[2],[1]] are also considered correct answers since per each level it does not matter the order on which elements are returned.
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```



**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`

---

## Analysis

&emsp;&emsp;题目给出一颗二叉树，要求按照一定的顺序去遍历：先遍历叶子节点，然后把叶子节点删掉，重复执行直到树为空。由于要先从叶子节点开始处理，所以很自然就会想到基于后序遍历去做，至于具体怎么做套了模板再说。从答案可以看出来，属于第一层叶子节点的放在同一个数组，属于第二层叶子节点的放在同一个数组，所以我们要标记出每个节点属于第几层叶子节点。那这个不就和算二叉树最大深度是一样的逻辑吗？从下到上，每个节点返回自己的高度，高度相同说明是同一层叶子。

## Solution

&emsp;&emsp;因为这里每层节点要放在同一个数组，所以在放入前要检查下是否存在该层的数组，下标是高度-1。比如高度为1的是最初的叶子节点，所以应该在下标为0的数组。

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
    vector<vector<int>> findLeaves(TreeNode* root) {
        traverse(root);
        return result;
    }
private:
    vector<vector<int>> result;
    int traverse(TreeNode* root) {
        if (!root) {
            return 0;
        }
        
        int l = traverse(root->left);
        int r = traverse(root->right);
        
        int cur = max(l, r) + 1;
        
        if (result.size() < cur) {
            result.push_back({});
        }
        
        result[cur - 1].push_back(root->val);
        return cur;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树一个比较少见的遍历方法，但其实就是基于后序遍历。这道题目的分享到这里，感谢你的支持！

