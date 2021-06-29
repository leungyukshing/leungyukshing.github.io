---
title: LeetCode解题报告（351) -- 589. N-ary Tree Preorder Traversal
tags:
  - LeetCode
mathjax: true
abbrlink: 60692
date: 2021-04-25 13:13:41
---

## Problem

Given the `root` of an n-ary tree, return *the preorder traversal of its nodes' values*.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [1,3,5,6,2,4]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [1,2,3,6,7,11,14,4,8,12,5,9,13,10]
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `0 <= Node.val <= 104`
- The height of the n-ary tree is less than or equal to `1000`.

 

**Follow up:** Recursive solution is trivial, could you do it iteratively?

------

## Analysis

&emsp;&emsp;N叉树的前序遍历，树的遍历无非就是递归和循环。对于二叉树来说，递归的解法更加简单清晰，但是对于N叉树来说，使用循环可能比较容易。其实思路与二叉树的一样的，还是借助栈来做。

&emsp;&emsp;每处理一个节点时，把它的子节点都压入栈中，但是这里注意顺序，因为遍历的顺序是从左到右，所以压入的顺序是从右到左。按照这个逻辑处理直至栈中没有节点即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> result;
        stack<Node*> s;
        if (!root) {
            return result;
        }
        
        s.push(root);
        while (!s.empty()) {
            Node* node = s.top();
            s.pop();
            result.push_back(node->val);
            int size = node->children.size();
            for (int i = size - 1; i >= 0; i--) {
                s.push(node->children[i]);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质上考察的是二叉树的前序遍历非递归实现，原理是使用栈来解决。N叉树和二叉树处理起来没有什么不同。这道题目的分享到这里，感谢你的支持！
