---
title: LeetCode解题报告（442）-- 210. Course Schedule II
mathjax: true
date: 2021-12-23 16:21:27
---

## Problem

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

<!-- more -->

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input: numCourses = 1, prerequisites = []
Output: [0]
```

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- All the pairs `[ai, bi]` are **distinct**.

------

## Analysis

&emsp;&emsp;这道这么经典的题目居然一直没有出题解，今天就来和大家分享一下。这道题目实际上是拓扑排序，通过这道题目，也能够彻底掌握拓扑排序的技巧。对于拓扑排序，我个人的套路是采用BFS。首先我们需要有两样东西，一个是建图，另一个是`indegree`。建图这里就不详细说了，我就说一下`indegree`是什么，这个一个统计入度的map（使用map还是数组看题目而定），它用来统计每个节点的入度，这个信息对我们BFS至关重要。**我们是否把某个节点放入到队列中的原则就是看它的入度是否为0。**

&emsp;&emsp;比如一开始有一些节点它只指向其他节点，这些节点的入度肯定就是0，所以可以放入队列。然后每处理一个节点，我们去查看它们的`neighbors`，我们先减去这些`neighbors`的入度，相当于遍历完一个节点后，把它从图里面删掉，所以它指向别人的边也要删掉，如果删掉后入度为0，说明这个点现在在图里面是最大的，于是就放入到队列中。一直这样处理到最后得到的顺序就是拓扑排序的顺序。

## Solution

&emsp;&emsp;需要注意，可能有多个节点同时指向一个节点，因此这里还是要用一个`visited`数组去记录已访问的节点。同时，这里还存在一种情况，如果存在环的话，就无法给出排序结果。判断环的方法很简单，遍历完之后，如果存在换说明有多于1个节点的入度一直都是2，无法加入到队列中。我们只需要在最后判断排序结果的size和节点的数量是否一致即可。

------

## Code

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        unordered_map<int, int> indegree;
        for (int i = 0; i < numCourses; i++) {
            indegree[i] = 0;
        }
        
        for (vector<int> &pre: prerequisites) {
            graph[pre[1]].push_back(pre[0]);
            indegree[pre[0]]++;
        }
        vector<bool> visited(numCourses, false);
        queue<int> q;
        for (auto &[k, v]: indegree) {
            if (v == 0) {
                q.push(k);
                visited[k] = true;
            }
        }
        
        vector<int> result;
        
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int cur = q.front();
                q.pop();
                result.push_back(cur);
                for (int &next: graph[cur]) {
                    if (visited[next]) {
                        continue;
                    }
                    if (--indegree[next] == 0) {
                        q.push(next);
                    }
                }
            }
        }
        if (result.size() == numCourses) {
            return result;
        } else {
            return {};
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目确实很经典，也是在BFS的基础上，添加了一个`indegree`信息去帮我们做拓扑排序，是一道经典好题。这道题目的分享到这里，感谢你的支持！
