---
title: LeetCode解题报告（447）-- 1530. Number of Good Leaf Nodes Pairs
mathjax: true
date: 2021-12-24 22:07:14
---

## Problem

You are given the `root` of a binary tree and an integer `distance`. A pair of two different **leaf** nodes of a binary tree is said to be good if the length of **the shortest path** between them is less than or equal to `distance`.

Return *the number of good leaf node pairs* in the tree.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/07/09/e1.jpg)

```
Output: 1
Explanation: The leaf nodes of the tree are 3 and 4 and the length of the shortest path between them is 3. This is the only good pair.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/07/09/e2.jpg)

```
Input: root = [1,2,3,4,5,6,7], distance = 3
Output: 2
Explanation: The good pairs are [4,5] and [6,7] with shortest path = 2. The pair [4,6] is not good because the length of ther shortest path between them is 4.
```

**Example 3:**

```
Input: root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
Output: 1
Explanation: The only good pair is [2,5].
```

**Constraints:**

- The number of nodes in the `tree` is in the range `[1, 210].`
- `1 <= Node.val <= 100`
- `1 <= distance <= 10`

------

## Analysis

&emsp;&emsp;这又是一道二叉树的题目，题目定义两个叶子节点的距离如果小于`distance`就是good node pair，要求统计有多少个这种pair。这和上面那题其实本质上很类似，某个节点需要左边子节点的信息，也需要右边子节点的信息，所以递归函数返回一个数组，数组的内容是该子树叶子节点的深度。如果只有一个叶子节点，那么就返回`{1}`；如果有两个叶子节点，一个深度为1，一个深度为2，那么就返回`{1, 2}`。

&emsp;&emsp;那么对于某个节点，它要怎么使用左右子树返回的信息呢？把这些左右叶子节点的深度相加就是叶子节点的距离，而且一定是最小的。为什么呢？因为当这个节点能处理到这两个叶子节点（分别在左右），说明它是最近的parent。然后我们把左边的和右边的都加一边，小于`distance`的就是符合要求的，结果+1。加完之后，把左右的叶子节点深度合在一起，返回给再上一层。

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
    int countPairs(TreeNode* root, int distance) {
        result = 0;
        dfs(root, distance);
        return result;
    }
private:
    int result;
    vector<int> dfs(TreeNode* root, int distance) {
        if (!root) {
            return {};
        }
        
        vector<int> p;
        auto left = dfs(root->left, distance);
        auto right = dfs(root->right, distance);
        
        if (left.size() == 0 & right.size() == 0) {
            p.push_back(1);
            return p;
        }
        
        for (int i = 0; i < left.size(); ++i) {
            for (int j = 0; j < right.size(); ++j) {
                if (left[i] + right[j] <= distance) {
                    result++;
                }
            }
        }
        
        for (int i = 0; i < left.size(); ++i) {
            left[i]++;
            p.push_back(left[i]);
        }
        
        for (int j = 0; j < right.size(); ++j) {
            right[j]++;
            p.push_back(right[j]);
        }
        return p;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目也是二叉树中有关路径长度的题目，也是递归函数需要返回多个值，这里返回的是叶子的深度。这道题目的分享到这里，感谢你的支持！
