---
title: LeetCode解题报告（287）-- 71. Simplify Path
tags:
  - LeetCode
mathjax: true
date: 2021-02-10 01:46:11

---

## Problem

Given a string `path`, which is an **absolute path** (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified **canonical path**.

In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (i.e. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.

The **canonical path** should have the following format:

- The path starts with a single slash `'/'`.
- Any two directories are separated by a single slash `'/'`.
- The path does not end with a trailing `'/'`.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`)

Return *the simplified **canonical path***.

<!-- more -->

**Example 1:**

```
Input: path = "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```

**Example 2:**

```
Input: path = "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```

**Example 3:**

```
Input: path = "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

**Example 4:**

```
Input: path = "/a/./b/../../c/"
Output: "/c"
```

**Constraints:**

- `1 <= path.length <= 3000`
- `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
- `path` is a valid absolute Unix path.

------

## Analysis

&emsp;&emsp;这是一道字符串处理的题目，给出一个路径，要求对其进行简化。这类题目其实没有特别好的算法去解决，主要是把握好规律，选择合适的数据结构。我们先来看有哪几种情况：

1. 重复的`/`需要简化为1个，同时我们需要用`/`去切分字符串；
2. 遇到`..`时，需要把前面一个路径给去除掉，因为返回了上一级；
3. 遇到`.`时不做处理，因为是当前目录

&emsp;&emsp;所以我们的处理思路就是把原字符串用`/`进行切割，对切割后的每一块进行处理，最后再把处理后的结果拼接起来即可。

------

## Solution

&emsp;&emsp;这里我选用了vector，实际上选用stack也是一样的，因为只会在末尾进行操作。但是vector的好处是最后拼接结果的时候比较方便。

------

## Code

```c++
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> v;
        int i = 0;
        int size = path.size();
        while (i < size) {
            while (path[i] == '/' && i < size) {
                i++;
            }
            if (i == size) {
                break;
            }
            int begin = i;
            while (path[i] != '/' && i < size) {
                i++;
            }
            int end = i - 1;
            string present = path.substr(begin, end - begin + 1);
            if (present == "..") {
                if (!v.empty()) {
                    v.pop_back();
                }
            }
            else if (present != ".") {
                v.push_back(present);
            }
        }
        if (v.empty()) {
            return "/";
        }
        string result = "";
        for (int i = 0; i < v.size(); i++) {
            result += '/' + v[i];
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是字符串类型中比较复杂的处理题目，包含的情况比较多，但是在算法层面上看是比较简单的。这道题目的分享到这里，谢谢您的支持！
