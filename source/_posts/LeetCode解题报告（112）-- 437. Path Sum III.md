---
title: LeetCode解题报告（112）-- 437. Path Sum III
tags:
  - LeetCode
date: 2020-08-09 08:47:38
mathjax: true

---

## Problem

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

<!-- more -->

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

------

## Analysis

&emsp;&emsp;关于二叉树路径求和的变种题目，题目给定一个数，要求计算路径和为该数的路径数量，需要注意，这里的路径不需要从根出发，但必须是从父节点到子节点的。因为是遍历的题目，同时有父子节点的顺序关系，所以我们依然考虑最常用的**先序遍历**。然后我们再来看如何去计算路径和。

&emsp;&emsp;路径和的计算其实不复杂，在先序遍历的前提下，计算从根节点到某个子节点并不困难，因为题目不限定必须从根节点出发，所以在得到从根节点出发的路径后，我们只需要在这条路径里面，**从上到下**每次减少一个节点，去计算剩下的路径和是否与题目给的一样就可以了。当节点本身完成递归后，把自己退出路径。

------

## Solution

&emsp;&emsp;在递归过程中维护几个信息：

1. 路径数量；
2. 当前路径和`currentSum`：这个变量记录的是从根节点出发到某一个节点的路径和。在剔除节点的时候，我们只需要每次用`currentSum`减去被剔除的节点的值即可；
3. 路径：使用vector来存储从根节点到某一个节点的路径。当开始剔除节点的时候，从头（根节点）开始剔除，注意不能剔除掉节点本身，所以遍历到`size - 2`就可以了。

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
    int pathSum(TreeNode* root, int sum) {
        vector<TreeNode*> path;
        int result = 0;
        int currentSum = 0;
        
        helper(root, path, currentSum, sum, result);
        return result;
    }
    
    void helper(TreeNode* node, vector<TreeNode*> path, int currentSum, const int sum, int& result) {
        if (!node) {
            return;
        }
        
        currentSum += node->val;
        path.push_back(node);
        
        int size = path.size();
        int temp = currentSum;
        
        if (temp == sum) {
            result++;
        }
        for (int i = 0; i < size - 1; i++) {
            temp -= path[i]->val;
            if (temp == sum) {
                result++;
            }
        }
        
        helper(node->left, path, currentSum, sum, result);
        helper(node->right, path, currentSum, sum, result);
        path.pop_back();
    }
};
```

------

## Summary

&emsp;&emsp;二叉树遍历的题目如果有明显的父子关系，大多数还是使用先序遍历解决。在解决路径和问题的时候，可以多考虑使用vector把路径信息记录下来，放到递归的过程中。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
