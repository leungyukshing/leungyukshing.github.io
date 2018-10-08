---
title: LeetCode解题报告（七）-- 895. Maximum Frequency Stack
tags:
  - LeetCode
abbrlink: 11927
date: 2018-09-17 15:51:34
---
## Problem
Implement `FreqStack`, a class which simulates the operation of a stack-like data structure.

`FreqStack` has two functions:
  + `push(int x)`, which pushes an integer `x` onto the stack.
  + `pop()`, which **removes** and returns the most frequent element in the stack.
    + If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.

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

**Note :**
  + Calls to `FreqStack.push(int x)` will be such that `0 <= x <= 10^9`.
  + It is guaranteed that `FreqStack.pop()` won't be called if the stack has zero elements.
  + The total number of `FreqStack.push` calls will not exceed `10000` in a single test case.
  + The total number of `FreqStack.pop` calls will not exceed `10000` in a single test case.
  + The total number of `FreqStack.push` and `FreqStack.pop` calls will not exceed `150000` across all test cases.


## Analysis
&emsp;&emsp;本题涉及到了数据结构中一个很重要的知识点--**栈**。栈的基本思想是**FIFO**，本题对`FreqStack`重新定义，要求在**FIFO**的基础上，还要考虑元素出现的次数。解决这个问题的要点在于，我要知道当前出现最高的频度。除此之外，从出栈的角度来说，判定的条件的优先级是：**出现频率>栈中先后顺序**。我的解决方法是，在存储的时候就直接按照这个优先级，分级存储。
## Solution
&emsp;&emsp;动态跟踪当前出现的最高频度，是当每一次插入新元素后，相应地检查的。因此我们需要一个map来记录每个元素出现的次数。
&emsp;&emsp;分级存储的实现也是充分利用map的特性，将key定义为频度（frequency），其value是一个栈。这就是说对于`key = x`，只要元素A出现了x次，那么他就会插入到这个栈中。对于`push操作`，我们可以通过先在记录出现频率的map中查找到某元素的出现次数`count`，以此作为key，到分级存储的map中找到对应的栈，插入即可。而对于`pop操作`，因为我们动态记录了出现的最大频度`max`，因此只需要以`max`作为key，在分级存储的map中找到相应位置的栈，`pop`出栈顶元素即可。
&emsp;&emsp;**特别注意**，`pop`后如果栈为空，就代表出现该频度的元素已经不存在了，这个时候需要将`max`-1。

## Code
```C++
class FreqStack {
public:
    FreqStack() {
        max = 0;
    }

    void push(int x) {
        counter[x]++;
        int times = counter[x];
        // update max
        max = counter[x] > max ? counter[x] : max;
        map1[times].push(x);

    }

    int pop() {
        int result = map1[max].top();
        map1[max].pop();
        // update max if the stack is empty
        if (map1[max].empty()) {
            max--;
        }
        counter[result]--;

        return result;
    }

private:
    map<int, stack<int>> map1;
    map<int, int> counter;
    int max;
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 */
```
**运行时间：**约172ms，超过65.12%的CPP代码。

## Summary
&emsp;&emsp;这道题是对栈的一个创新，本质上还是对栈的使用。在统计数字或字母出现次数的时候，map是一个很好的工具，因为它维护的**key是唯一的**，且是**自动排序的**，因此以后遇到类似的题目应该首先考虑使用map，而不是vector。本题的分析就到这里，谢谢！
