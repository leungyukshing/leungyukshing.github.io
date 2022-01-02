---
title: LeetCode解题报告（490）-- 919. Complete Binary Tree Inserter
mathjax: true
date: 2022-01-01 19:13:18
---

## Problem

A **complete binary tree** is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.

Design an algorithm to insert a new node to a complete binary tree keeping it complete after the insertion.

Implement the `CBTInserter` class:

- `CBTInserter(TreeNode root)` Initializes the data structure with the `root` of the complete binary tree.
- `int insert(int v)` Inserts a `TreeNode` into the tree with value `Node.val == val` so that the tree remains complete, and returns the value of the parent of the inserted `TreeNode`.
- `TreeNode get_root()` Returns the root node of the tree.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/08/03/lc-treeinsert.jpg)

```
Input
["CBTInserter", "insert", "insert", "get_root"]
[[[1, 2]], [3], [4], []]
Output
[null, 1, 2, [1, 2, 3, 4]]

Explanation
CBTInserter cBTInserter = new CBTInserter([1, 2]);
cBTInserter.insert(3);  // return 1
cBTInserter.insert(4);  // return 2
cBTInserter.get_root(); // return [1, 2, 3, 4]
```



**Constraints:**

- The number of nodes in the tree will be in the range `[1, 1000]`.
- `0 <= Node.val <= 5000`
- `root` is a complete binary tree.
- `0 <= val <= 5000`
- At most `104` calls will be made to `insert` and `get_root`.

---

## Analysis

&emsp;&emsp;题目给出一颗二叉树，要求实现一个类，支持向二叉树中插入节点，并且要求插入完之后也是一颗完全二叉树（Complete Binary Tree）。完全二叉树插入操作的特点就是优先插入左节点，如果左节点不为空，就插入到右节点。那么我们应该如何选取插入到那个节点呢？遵循从上到下、从左到右的原则，所以需要在初始化的时候就先做一次BFS，把只有一个叶子的节点和叶子节点都添加到一个队列中，然后每次插入的时候，从队头拿出节点，按照上面说的规则插入，如果插入完该节点有两个叶子，说明该节点已经满了。最后记得把新插入的节点加到队列中。

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
class CBTInserter {
public:
    CBTInserter(TreeNode* root) {
        this->root = root;
        
        // bfs
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            if (!node->left || !node->right) {
                dequeue.push(node);
            }
            if (node->left) {
                q.push(node->left);
            }
            if (node->right) {
                q.push(node->right);
            }
        }
    }
    
    int insert(int val) {
        //cout << "insert " << val << " " << dequeue.size() << endl;
        auto node = dequeue.front();
        TreeNode* newNode = new TreeNode(val);
        if (!node->left) {
            node->left = newNode;
        } else {
            //cout << "pop out" << endl;
            node->right = newNode;
            dequeue.pop();
        }
        //cout << "push in" << endl;
        dequeue.push(newNode);
        //cout << "return value" << node->val << endl;
        return node->val;
    }
    
    TreeNode* get_root() {
        return root;
    }
private:
    TreeNode* root;
    queue<TreeNode*> dequeue;
};

/**
 * Your CBTInserter object will be instantiated and called as such:
 * CBTInserter* obj = new CBTInserter(root);
 * int param_1 = obj->insert(val);
 * TreeNode* param_2 = obj->get_root();
 */
```

------

## Summary

&emsp;&emsp;这道题目实际上就是一个BFS加满二叉树的考察。对于满二叉树插入来说，重点就在于先插入到左子树，再插入到右子树，节点顺序是从上到下、从左到右，刚好是一个BFS遍历。这道题目的分享到这里，感谢你的支持！

