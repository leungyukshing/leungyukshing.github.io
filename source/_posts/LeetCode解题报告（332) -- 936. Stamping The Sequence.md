---
title: LeetCode解题报告（332) -- 936. Stamping The Sequence
tags:
  - LeetCode
mathjax: true
date: 2021-04-05 20:58:19

---

## Problem

You want to form a `target` string of **lowercase letters**.

At the beginning, your sequence is `target.length` `'?'` marks.  You also have a `stamp` of lowercase letters.

On each turn, you may place the stamp over the sequence, and replace every letter in the sequence with the corresponding letter from the stamp.  You can make up to `10 * target.length` turns.

For example, if the initial sequence is "?????", and your stamp is `"abc"`,  then you may make "abc??", "?abc?", "??abc" in the first turn.  (Note that the stamp must be fully contained in the boundaries of the sequence in order to stamp.)

If the sequence is possible to stamp, then return an array of the index of the left-most letter being stamped at each turn.  If the sequence is not possible to stamp, return an empty array.

For example, if the sequence is "ababc", and the stamp is `"abc"`, then we could return the answer `[0, 2]`, corresponding to the moves "?????" -> "abc??" -> "ababc".

Also, if the sequence is possible to stamp, it is guaranteed it is possible to stamp within `10 * target.length` moves.  Any answers specifying more than this number of moves will not be accepted.

<!-- more -->

**Example 1:**

```
Input: stamp = "abc", target = "ababc"
Output: [0,2]
([1,0,2] would also be accepted as an answer, as well as some other answers.)
```

**Example 2:**

```
Input: stamp = "abca", target = "aabcaca"
Output: [3,0,1]
```

**Note:**

1. `1 <= stamp.length <= target.length <= 1000`
2. `stamp` and `target` only contain lowercase letters.

------

## Analysis

&emsp;&emsp;题目给出了一个目标字符串`target`和一个邮戳字符串`stamp`，问邮戳怎么盖才能产生目标字符串。从一个空白的字符串，去通过盖章产生目标字符串是比较困难的，因为这里组合的数量非常多，我们不妨转变一下思路，逆向求解。把题目转化为，从目标字符串`target`转化为一个空白的字符串，所以这里邮戳的作用就是消去某些字符。比如有字符串`abcd`，邮戳`abc`，那么邮戳盖在下标为0的位置，字符串就变成了`***d`了。

&emsp;&emsp;对于已经消失的字符，在邮戳中我们也不关心他们，所以邮戳也有相应的变化，如`abc` 的邮戳实际上可以变化为`ab*`、`a*b`、`*ab`、`a**`、`*b*`等等。然后我们再用这些变化后的邮戳，去和字符串做匹配。

&emsp;&emsp; 用一个完整的例子再演示一遍流程，对于字符串`abcb`和邮戳`abc`来说：

1. 在字符串`abc`中匹配到了`abc `，记录下标为0，所以直接替换成`***b`；
2. 邮戳开始产生变种，当产生了`*bc`时，在字符串匹配到了最后，记录下标为2，所以替换成`****`；
3. 因为字符串全部都变成了`*`，所以没有任何邮戳能够匹配到了，退出循环。
4. 此时操作的数组是`[0,2]`，因为我们是逆向操作，所以需要把数组翻转过来作为答案。

------

## Solution

&emsp;&emsp;在生成邮戳的变种时，需要注意不能产生全是`* `的邮戳去匹配。

------

## Code

```c++
class Solution {
public:
    vector<int> movesToStamp(string stamp, string target) {
        vector<int> res;
        int n = stamp.size(), total = 0;
        while (true) {
            bool isStamped = false;
            for (int size = n; size > 0; --size) {
                for (int i = 0; i <= n - size; ++i) {
                    string t = string(i, '*') + stamp.substr(i, size) + string(n - size - i, '*');
                    // cout << t << endl;
                    auto pos = target.find(t);
                    while (pos != string::npos) {
                        res.push_back(pos);
                        isStamped = true;
                        total += size;
                        fill(begin(target) + pos, begin(target) + pos + n, '*');
                        pos = target.find(t);
                    }
                }
            }
            if (!isStamped) break;
        }
        reverse(res.begin(), res.end());
        return total == target.size() ? res : vector<int>();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是字符串中比较难的题目，涉及到字符串匹配、生成等问题。对于匹配，我们可以借用STL的方法，对于生成，就需要自己慢慢尝试debug，尤其注意边界条件。这道题目的分享到这里，感谢你的支持！
