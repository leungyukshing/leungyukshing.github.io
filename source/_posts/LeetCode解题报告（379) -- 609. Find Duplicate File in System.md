---
title: LeetCode解题报告（379) -- 609. Find Duplicate File in System
tags:
  - LeetCode
mathjax: true
abbrlink: 37913
date: 2021-05-30 22:15:07
---

## Problem

Given a list `paths` of directory info, including the directory path, and all the files with contents in this directory, return *all the duplicate files in the file system in terms of their paths*. You may return the answer in **any order**.

A group of duplicate files consists of at least two files that have the same content.

A single directory info string in the input list has the following format:

- `"root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)"`

It means there are `n` files `(f1.txt, f2.txt ... fn.txt)` with content `(f1_content, f2_content ... fn_content)` respectively in the directory "`root/d1/d2/.../dm"`. Note that `n >= 1` and `m >= 0`. If `m = 0`, it means the directory is just the root directory.

The output is a list of groups of duplicate file paths. For each group, it contains all the file paths of the files that have the same content. A file path is a string that has the following format:

- `"directory_path/file_name.txt"`

<!-- more -->

**Example 1:**

```
Input: paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)","root 4.txt(efgh)"]
Output: [["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]
```

**Example 2:**

```
Input: paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)"]
Output: [["root/a/2.txt","root/c/d/4.txt"],["root/a/1.txt","root/c/3.txt"]]
```



**Constraints:**

- `1 <= paths.length <= 2 * 104`
- `1 <= paths[i].length <= 3000`
- `1 <= sum(paths[i].length) <= 5 * 105`
- `paths[i]` consist of English letters, digits, `'/'`, `'.'`, `'('`, `')'`, and `' '`.
- You may assume no files or directories share the same name in the same directory.
- You may assume each given directory info represents a unique directory. A single blank space separates the directory path and file info.

 

**Follow up:**

- Imagine you are given a real file system, how will you search files? DFS or BFS?
- If the file content is very large (GB level), how will you modify your solution?
- If you can only read the file by 1kb each time, how will you modify your solution?
- What is the time complexity of your modified solution? What is the most time-consuming part and memory-consuming part of it? How to optimize?
- How to make sure the duplicated files you find are not false positive?

------

## Analysis

&emsp;&emsp;这道题目是字符串处理的题目，题目看上去比较复杂，但实际上就是对字符串的切割。题目给出了一个字符串数组，每一个字符串都是包含三部分：文件路径，后面是若干个文件名+文件内容。最后题目要求返回的是，按照文件内容分类，把相同文件内容的路径合到一个数组。

&emsp;&emsp;首先是利用空格，把文件路径截取出来，然后利用`(`把文件名拿到，最后再拿到文件内容。这里用一个map存储，利用文件内容作为key，value是路径，然后把文件路径加上文件名存进去。

------

## Solution

&emsp;&emsp;C++处理字符串时可以多用streamstring来切割，同时还可以使用`find_last_of`函数。

------

## Code

```c++
class Solution {
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        vector<vector<string>> res;
        unordered_map<string, vector<string>> m;
        for (string path : paths) {
            istringstream is(path);
            string pre = "", t = "";
            is >> pre;
            while (is >> t) {
                int idx = t.find_last_of('(');
                string dir = pre + "/" + t.substr(0, idx);
                string content = t.substr(idx + 1, t.size() - idx - 2);
                m[content].push_back(dir);
            }
        }
        for (auto a : m) {
            if (a.second.size() > 1)res.push_back(a.second);
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目理解起来有点复杂，但是整体的思路和方法都是很简单的，就是字符串的切割。这道题目的分享到这里，感谢你的支持！
