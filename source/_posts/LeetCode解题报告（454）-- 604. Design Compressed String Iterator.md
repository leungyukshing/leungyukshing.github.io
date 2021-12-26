---
title: LeetCode解题报告（454）-- 604. Design Compressed String Iterator
mathjax: true
date: 2021-12-25 23:30:25
---

## Problem

Design and implement a data structure for a compressed string iterator. The given compressed string will be in the form of each letter followed by a positive integer representing the number of this letter existing in the original uncompressed string.

Implement the StringIterator class:

- `next()` Returns **the next character** if the original string still has uncompressed characters, otherwise returns a **white space**.
- `hasNext()` Returns true if there is any letter needs to be uncompressed in the original string, otherwise returns `false`.

<!-- more -->

**Example 1:**

```
Input
["StringIterator", "next", "next", "next", "next", "next", "next", "hasNext", "next", "hasNext"]
[["L1e2t1C1o1d1e1"], [], [], [], [], [], [], [], [], []]
Output
[null, "L", "e", "e", "t", "C", "o", true, "d", true]

Explanation
StringIterator stringIterator = new StringIterator("L1e2t1C1o1d1e1");
stringIterator.next(); // return "L"
stringIterator.next(); // return "e"
stringIterator.next(); // return "e"
stringIterator.next(); // return "t"
stringIterator.next(); // return "C"
stringIterator.next(); // return "o"
stringIterator.hasNext(); // return True
stringIterator.next(); // return "d"
stringIterator.hasNext(); // return True
```

**Constraints:**

- `1 <= compressedString.length <= 1000`
- `compressedString` consists of lower-case an upper-case English letters and digits.
- The number of a single character repetitions in `compressedString` is in the range `[1, 10^9]`
- At most `100` calls will be made to `next` and `hasNext`.

---

## Analysis

&emsp;&emsp;这是一道iterator的题目，也是实现`next`和`hasNext`两个方法。这种题目实现的方法有很多，比如在构造函数就处理好，或者按需计算等等。通常来说面试都是需要按需计算，这里我也用这种解法。首先我要把原始的`compressedString`存下来，然后我还有两个变量，一个是`idx`，指向当前的字符，还有一个是`count`，表明当前字符还有多少个。

&emsp;&emsp;每次我都从`idx + 1`开始计算，把`count`计算出来，注意这里的`count`可能是多位的，所以要用循环。然后每次`next`时，`count`自减1，如果减到为0，则需要移动`idx`。`idx`的移动需要跳过所有的数字，直到下一个字符。

&emsp;&emsp;`hasNext`就比较简单了，直接判断`idx`和字符串的长度就可以了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class StringIterator {
public:
    StringIterator(string compressedString) {
        str = compressedString;
        idx = 0;
        int i = 1;
        count = 0;
        while (i < str.size()) {
            if (str[i] >= '0' && str[i] <= '9') {
                count = count * 10 + (str[i] - '0');
                ++i;
            } else {
                break;
            }
        }
    }
    
    char next() {
        if (idx >= str.size()) {
            return ' ';
        }
        char ch = str[idx];
        if (--count == 0) {
            //cout << "renew count " << idx << endl;
            idx++;
            while (idx < str.size() && str[idx] >= '0' && str[idx] <= '9') {
                idx++;
            }
            
            int i = idx + 1;
            while (i < str.size()) {
                if (str[i] >= '0' && str[i] <= '9') {
                    count = count * 10 + (str[i] - '0');                                    
                    ++i;
                } else {
                    break;
                }
            }
        }
        return ch;
    }
    
    bool hasNext() {
        return idx < str.size();
    }
private:
    string str;
    int idx;
    int count;
};

/**
 * Your StringIterator object will be instantiated and called as such:
 * StringIterator* obj = new StringIterator(compressedString);
 * char param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

------

## Summary

&emsp;&emsp;这道题目还算是比较常规的iterator题目，算是见多一种类型。这道题目的分享到这里，感谢你的支持！
