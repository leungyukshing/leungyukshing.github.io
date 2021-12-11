---
title: LeetCode解题报告（409) -- 282. Expression Add Operators Station
mathjax: true
abbrlink: 65210
date: 2021-09-18 10:38:41
---

## Problem

Given a string `num` that contains only digits and an integer `target`, return *all possibilities to add the binary operators* `'+'`, `'-'`, *or* `'*'` *between the digits of* `num` *so that the resultant expression evaluates to the* `target` *value*.

<!-- more -->

**Example 1:**

```
Input: num = "123", target = 6
Output: ["1*2*3","1+2+3"]
```

**Example 2:**

```
Input: num = "232", target = 8
Output: ["2*3+2","2+3*2"]
```

**Example 3:**

```
Input: num = "105", target = 5
Output: ["1*0+5","10-5"]
```

**Example 4:**

```
Input: num = "00", target = 0
Output: ["0*0","0+0","0-0"]
```

**Example 5:**

```
Input: num = "3456237490", target = 9191
Output: []
```

**Constraints:**

- `1 <= num.length <= 10`
- `num` consists of only digits.
- `-231 <= target <= 231 - 1`

------

## Analysis

&emsp;&emsp;题目给出一个仅由数字组成的字符串，要求在数字之间插入`+`、`-`和`*`这三种运算符，使得运算结果等于给出的`target`。因为题目要求返回全部的结果，所以很明显思路就是递归+回溯。主要的思路有了，比较难解决的点就是如何去判断表达式是否等于target。计算算术表达式之前也碰到过类似的题目，一般都是采用栈的方式，但这里我们希望不是到最后一步才进行运算，而是在过程中就进行运算。比如`2+3*2`，我们先计算了`2+3`，再碰到`2`时，我们需要计算三种情况：

+ `+2`：加法的优先级最低，所以可以直接相加；
+ `-2`：减法的优先级也是最低，也可以直接相加；
+ `*2`：乘法的优先级较高，需要特殊处理。我们需要记录前一次变更的大小`diff`，在这里`diff = 3`。然后用`curNum - diff = 5 - 3 = 2`，得到上上次的值，然后再把上次变更的值与2相乘，最后得到结果`curNum - diff + diff * 2`

&emsp;&emsp;按照这种方法我们就可以按照顺序从左到右进行计算，而不需要到最后再一次过进行计算。判断符合要求的条件是当剩余的`num`长度为0并且当前的值等于`target`，则添加到答案中。

&emsp;&emsp;这里还需要注意一种情况，就是数字开头有多余的0，所以当我们切分字符串时，发现长度大于1且第一个字符是0，说明是非法的，可以直接跳过后续的递归。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<string> addOperators(string num, int target) {
        string out = "";
        vector<string> result;
        helper(num, target, 0, 0, out, result);
        return result;
    }
private:
    void helper(string num, int target, long diff, long curNum, string out, vector<string> &result) {
        //cout << num << " " << curNum << endl;
        if (num.size() == 0 && curNum == target) {
            result.push_back(out);
            return;
        }
        
        for (int i = 1; i <= num.size(); i++) {
            string cur = num.substr(0, i);
            if (cur.size() > 1 && cur[0] == '0') {
                break;
            }
            string next = num.substr(i);
            if (out.size() > 0) {
                helper(next, target, stoll(cur), stoll(cur) + curNum, out + "+" + cur, result);
                helper(next, target, -stoll(cur), curNum - stoll(cur), out + "-" + cur, result);
                helper(next, target, diff * stoll(cur), (curNum - diff) + diff * stoll(cur), out + "*" + cur, result);
            } else {
                helper(next, target, stoll(cur), stoll(cur), cur, result);
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目主要的思路是递归+回溯，难点在于算术运算。这里给出了一种新的思路去从左往右计算算术表达式。这道题目的分享到这里，感谢你的支持！

