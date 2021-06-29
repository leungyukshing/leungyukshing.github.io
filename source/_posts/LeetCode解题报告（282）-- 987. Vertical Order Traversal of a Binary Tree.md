---
title: LeetCode解题报告（282）-- 987. Vertical Order Traversal of a Binary Tree
tags:
  - LeetCode
mathjax: true
abbrlink: 31207
date: 2021-02-08 17:10:02
---

## Problem

Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(row, col)`, its left and right children will be at positions `(row + 1, col - 1)` and `(row + 1, col + 1)` respectively. The root of the tree is at `(0, 0)`.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return *the **vertical order traversal** of the binary tree*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation:
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

```
Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

```
Input: root = [1,2,3,4,6,5,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

------

## Analysis

&emsp;&emsp;题目的意思很简单，对二叉树进行竖直方向上的遍历。前序、中序、后序遍历，层次遍历我们都做过很多次，但是竖直方向上的遍历就第一次做。先来看什么样的元素属于同一列。

&emsp;&emsp;根节点的横坐标为0，左子树-1，右子树+1，递归下去就能得到每个节点的横坐标。我们需要把横坐标较小的节点先遍历，所以这里就存在一个顺序问题，单纯用queue或者stack肯定是不行，因为没有办法按照横坐标来排序。这里就需要用到特殊的数据结构——优先队列了，使用优先队列能够维护横坐标最小的节点。然后遍历时，把相同横坐标的都放到同一个队列中，这样最后拿出来的时候就是按照题目要求了。

------

## Solution

&emsp;&emsp;这里用到了优先队列，因为我们需要自定义在队列中的优先级，所以要重写比较的函数。由于优先队列实现的特殊性，它的队头实际上指向的是队尾，所以比较的逻辑是相反的。当横坐标不同时，横坐标大的排前面；横坐标相同时，值小的放前面。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        queue<pair<TreeNode*, int>> Q; Q.push({root, 0}); // node: x
        class cmp {
        public:
            bool operator()(const pair<int, int>& a, const pair<int, int>& b) { // (level, val)
                if (a.first == b.first) return a.second > b.second; // level一样值小的排前边 (priority_queue逻辑是反的)
                return a.first < b.first; // level大的排前边 
            }
        };
        map<int, priority_queue<pair<int, int>, vector<pair<int, int>>, cmp>> m; // x: pq(level, val) x从小到大 level从大到小
        m[0].push({0, root -> val});
        int level = -1;
        while (!Q.empty()) {
            int size = Q.size();
            for (int i = 0; i < size; i++) {
                auto node = Q.front().first; auto x = Q.front().second; Q.pop();
                if (node -> left) {
                    Q.push({node -> left, x - 1});
                    m[x - 1].push({level, node -> left -> val});
                }
                if (node -> right) {
                    Q.push({node -> right, x + 1});
                    m[x + 1].push({level, node -> right -> val});
                }
            }
            level--;
        }
        vector<vector<int>> res;
        for (auto& [x, pq]: m) {
            vector<int> cur;
            while (!pq.empty()) {
                auto p = pq.top(); pq.pop();
                cur.emplace_back(p.second);
            }
            res.emplace_back(cur);
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树和优先队列的配合使用，还考察了对优先队列的自定义。总的来说思路不难，但是coding量比较大，需要有一定的熟练程度才能在短时间内解决这道题目。这道题这道题目的分享到这里，谢谢您的支持！
