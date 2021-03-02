---
title: LeetCode解题报告（304)-- 895. Maximum Frequency Stack
tags:
  - LeetCode
mathjax: true
date: 2021-03-03 01:22:28

---

## Problem

Implement `FreqStack`, a class which simulates the operation of a stack-like data structure.

`FreqStack` has two functions:

- `push(int x)`, which pushes an integer `x` onto the stack.
- `pop()`, which **removes** and returns the most frequent element in the stack.
  - If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.

<!-- more -->

**Example 1:**

```
Input: 
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
Output: [null,null,null,null,null,null,null,5,7,5,4]
Explanation:
After making six .push operations, the stack is [5,7,5,7,4,5] from bottom to top.  Then:

pop() -> returns 5, as 5 is the most frequent.
The stack becomes [5,7,5,7,4].

pop() -> returns 7, as 5 and 7 is the most frequent, but 7 is closest to the top.
The stack becomes [5,7,5,4].

pop() -> returns 5.
The stack becomes [5,7,4].

pop() -> returns 4.
The stack becomes [5,7].
```

**Note:**

- Calls to `FreqStack.push(int x)` will be such that `0 <= x <= 10^9`.
- It is guaranteed that `FreqStack.pop()` won't be called if the stack has zero elements.
- The total number of `FreqStack.push` calls will not exceed `10000` in a single test case.
- The total number of `FreqStack.pop` calls will not exceed `10000` in a single test case.
- The total number of `FreqStack.push` and `FreqStack.pop` calls will not exceed `150000` across all test cases.

------

## Analysis

&emsp;&emsp;题目要求我们实现一个频率栈，元素入栈后，出栈的顺序按照元素出现频率的多少分先后，频率高的先出栈，频率低的后出栈，频率相同时按照入栈的顺序进行出栈。我们先分析题目的特点，再来看应该使用什么数据结构。

&emsp;&emsp;首先整道题目的关键就是频率，统计数字出现的频率我们一般使用map，这样频率的问题就解决了。然后我们要考虑出栈的时候，需要怎么找到频率最高的元素。对于某个频率来说，它所对应的应该也是一个栈，例如`[1,1,1,2,3,3,4,4,4]`，频率为3的数字有两个，分别是1和4，所以这里也是一个map，key是频率，value是一个栈，包含了频率所对应的数字。

&emsp;&emsp;然后全局再维护一个当前最大频率，当入栈时，我们找到数字出现的频率，并自增1，同时把这个数字添加到这个频率下的栈；当出栈时，由于我们维护了全局最大的频率，所以直接用这个频率找到对应的栈，栈顶元素就是要返回的数字，同时，如果移除掉这个数字后栈为空，说明这个频率下的数字已经不存在了，所以最大频率要自减。

------

## Solution

&emsp;&emsp;频率对应栈也是使用一个map，value可以是栈，也可以是数组，这里我使用了vector，其本质和stack没有区别。

------

## Code

```c++
class FreqStack {
public:
    FreqStack() {
        
    }
    
    void push(int x) {
        mxFreq = max(mxFreq, ++freq[x]);
        m[freq[x]].push_back(x);
    }
    
    int pop() {
        int x = m[mxFreq].back(); 
        m[mxFreq].pop_back();
        if (m[freq[x]--].empty()) --mxFreq;
        return x;
    }
private:
    int mxFreq;
    unordered_map<int, int> freq;
    unordered_map<int, vector<int>> m;
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 */
```

------

## Summary

&emsp;&emsp;这是一道自定义特殊栈的设计题，这类题目并没有很统一的解题思路，只能是根据题目的要求自己想办法存储。而比较常用的一般是数组和map。这道题目的分享到这里，感谢你的支持！
