---
title: LeetCode解题报告（254）-- 1457. Pseudo-Palindromic Paths in a Binary Tree
tags:
  - LeetCode
mathjax: true
date: 2020-12-29 23:44:51

---

## Problem

Given a binary tree where node values are digits from 1 to 9. A path in the binary tree is said to be **pseudo-palindromic** if at least one permutation of the node values in the path is a palindrome.

*Return the number of **pseudo-palindromic** paths going from the root node to leaf nodes.*

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/05/06/palindromic_paths_1.png)

```
Input: root = [2,3,1,3,1,null,1]
Output: 2 
Explanation: The figure above represents the given binary tree. There are three paths going from the root node to leaf nodes: the red path [2,3,3], the green path [2,1,1], and the path [2,3,1]. Among these paths only red path and green path are pseudo-palindromic paths since the red path [2,3,3] can be rearranged in [3,2,3] (palindrome) and the green path [2,1,1] can be rearranged in [1,2,1] (palindrome).
```

**Example 2:**

![Example 2](https://assets.leetcode.com/uploads/2020/05/07/palindromic_paths_2.png)

```
Input: root = [2,1,1,1,3,null,null,null,null,null,1]
Output: 1 
Explanation: The figure above represents the given binary tree. There are three paths going from the root node to leaf nodes: the green path [2,1,1], the path [2,1,3,1], and the path [2,1]. Among these paths only the green path is pseudo-palindromic since [2,1,1] can be rearranged in [1,2,1] (palindrome).
```

**Example 3:**

```
Input: root = [9]
Output: 1
```

**Constraints:**

- The given binary tree will have between `1` and `10^5` nodes.
- Node values are digits from `1` to `9`.

------

## Analysis

&emsp;&emsp;这道题是回文数和二叉树的结合。这里有两个步骤，一个是获取所有的从root到leaf的路径，第二个是判断路径上的数字能否构成回文数。第一个问题比较好解决，用DFS即可。我们再来看第二个问题，如何判断路径上的数字能否构成回文数呢？有一个很简单的方法是，判断字符串中数字出现的次数为奇数的有多少个。如果是小于等于1个，那么肯定能组成回文数。根据这个规则，我们可以结合DFS来进行判断，当遍历到叶子节点的时候，我们就检查路径上数字出现的次数，如果不符合就返回0，如果符合就返回1，代表有一条路径。

------

## Solution

&emsp;&emsp;统计数字出现的次数用map也可以，这里因为只有十个数字，所以直接用数组也问题不大。因为这个统计的数据结构是共用的，所以特别注意dfs后需要把当前数字的出现次数减1。

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
    int pseudoPalindromicPaths (TreeNode* root) {
        return helper(root);
    }
private:
    int helper(TreeNode* root) {
        if (!root) {
            return 0;
        }
        ++counts[root->val];
        int c = 0;
        if (!root->left && !root->right) {
            int odds = 0;
            for (int i = 1; i <= 9; i++) {
                if (counts[i] & 1) {
                    odds++;
                }
            }
            // satisfy requirementss
            if (odds <= 1) {
                c = 1;
            }
        }
        int left = helper(root->left);
        int right = helper(root->right);
        --counts[root->val];
        return c + left + right;
    }
    vector<int> counts{vector<int>(10, 0)};
};
```

------

## Summary

&emsp;&emsp;这道题目是一道二叉树和字符串的结合，本质还是在二叉树DFS中做文章，只要把握住这个要点，解题的思路就有了。同时在处理回文数的时候，这里也用到了一些技巧。这道题这道题目的分享到这里，谢谢您的支持！
