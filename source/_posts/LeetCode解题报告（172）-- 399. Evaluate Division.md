---
title: LeetCode解题报告（172）-- 399. Evaluate Division
tags:
  - LeetCode
mathjax: true
abbrlink: 13270
date: 2020-09-28 00:27:19
---

## Problem

You are given `equations` in the format `A / B = k`, where `A` and `B` are variables represented as strings, and `k` is a real number (floating-point number). Given some `queries`, return *the answers*. If the answer does not exist, return `-1.0`.

The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

<!-- more -->

**Example 1:**

```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
```

**Example 2:**

```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```

**Example 3:**

```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```

**Constraints:**

- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= equations[i][0], equations[i][1] <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= queries[i][0], queries[i][1] <= 5`
- `equations[i][0], equations[i][1], queries[i][0], queries[i][1]` consist of lower case English letters and digits.

------

## Analysis

&emsp;&emsp;这道题目从数学上是一道非常简单的题目，题目给出一些方程，然后要求求解一些数学表达式。如果是用纸笔做，相信大家都能很快地做出来，但是要转化成代码计算，就比较复杂了。

&emsp;&emsp;分数的计算主要有几种情况：

1. 直接可以从给出的条件得到，如：已知`a/b`，求`a/b`；
2. 间接通过题目的条件计算得到，如：已知`a/b`、`b/c`，求`a/c`；
3. 在第2种情况下，有些还需要使用倒数运算，如：已知`a/b`、`b/c`，求`c/a`。

&emsp;&emsp;对于上面的三种情况，我们有不同的处理策略。第一种，我们只需要匹配到分子和分母和条件给出的一样，就能够得到答案；第二种，需要递归处理，匹配到分子`a`相同之后，紧接着用分母`b`去找其他分子为`b`的数，找到`b/c`之后，发现分母`c`和要求的分母是一样的，两个值相乘即得出结果；第三种，本质上的处理方式和第二种是一样的，但是没必要在运算的过程中去考虑分子和分母的位置，我们直接就把倒数的值当作是已知条件，所以当题目给出了`a/b`和`b/c`之后，实际上已经知道了`b/a`和`c/b`的值了。

------

## Solution

&emsp;&emsp;存储分数方面，这里使用了嵌套的map，因为map对分子和分母做匹配都比较方便。其次是在迭代的时候，因为要避免重复的运算，所以要用一个`visited`的map来做记录。

------

## Code

```c++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        int size = equations.size();
        for (int i = 0; i < size; i++) {
            m[equations[i][0]][equations[i][1]] = values[i];
            m[equations[i][1]][equations[i][0]] = 1.0 / values[i];
        }
        vector<double> result;
        for (auto q: queries) {
            unordered_set<string> visited;
            double t = helper(q[0], q[1], visited);
            if (t > 0) {
                result.push_back(t);                
            } else {
                result.push_back(-1);
            }
        }
        return result;
    }
private:
    double helper(string numerator, string denominator, unordered_set<string>& visited) {
        if (m[numerator].count(denominator)) {
            return m[numerator][denominator];
        }
        for (auto a: m[numerator]) {
            if (visited.count(a.first)) {
                continue;
            }
            visited.insert(a.first);
            double t = helper(a.first, denominator, visited);
            if (t > 0) {
                return t * a.second;
            }
        }
        return -1.0;
    }
    unordered_map<string, unordered_map<string, double>> m;
};
```

------

## Summary

&emsp;&emsp;这道题非常有意思，使用代码来求解数学方程，其中用到两个map的方式非常值得总结。这道题的分析到这里，谢谢！
