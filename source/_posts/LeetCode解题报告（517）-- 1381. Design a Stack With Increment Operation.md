---
title: LeetCode解题报告（517）-- 1381. Design a Stack With Increment Operation
mathjax: true
date: 2022-04-13 11:40:42

---

## Problem

Design a stack which supports the following operations.

Implement the `CustomStack` class:

- `CustomStack(int maxSize)` Initializes the object with `maxSize` which is the maximum number of elements in the stack or do nothing if the stack reached the `maxSize`.
- `void push(int x)` Adds `x` to the top of the stack if the stack hasn't reached the `maxSize`.
- `int pop()` Pops and returns the top of stack or **-1** if the stack is empty.
- `void inc(int k, int val)` Increments the bottom `k` elements of the stack by `val`. If there are less than `k` elements in the stack, just increment all the elements in the stack.

<!-- more -->

**Example 1:**

```
Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
Explanation
CustomStack customStack = new CustomStack(3); // Stack is Empty []
customStack.push(1);                          // stack becomes [1]
customStack.push(2);                          // stack becomes [1, 2]
customStack.pop();                            // return 2 --> Return top of the stack 2, stack becomes [1]
customStack.push(2);                          // stack becomes [1, 2]
customStack.push(3);                          // stack becomes [1, 2, 3]
customStack.push(4);                          // stack still [1, 2, 3], Don't add another elements as size is 4
customStack.increment(5, 100);                // stack becomes [101, 102, 103]
customStack.increment(2, 100);                // stack becomes [201, 202, 103]
customStack.pop();                            // return 103 --> Return top of the stack 103, stack becomes [201, 202]
customStack.pop();                            // return 202 --> Return top of the stack 102, stack becomes [201]
customStack.pop();                            // return 201 --> Return top of the stack 101, stack becomes []
customStack.pop();                            // return -1 --> Stack is empty return -1.
```



**Constraints:**

- `1 <= maxSize <= 1000`
- `1 <= x <= 1000`
- `1 <= k <= 1000`
- `0 <= val <= 100`
- At most `1000` calls will be made to each method of `increment`, `push` and `pop` each separately.

---

## Analysis

&emsp;&emsp;这是一道设计题，要求设计一个可以对栈中底部`k`个元素进行增加操作的栈。对于栈而言，操作栈顶的元素是容易的，但是操作栈底的元素是很麻烦的。所以第一个思路就是不直接使用栈的结构，而是用数组去实现一个栈。使用数组的好处在于可以很方便地修改栈底元素，我们把`idx = 0`视为栈底，数组的右端视为栈顶，依次操作即可。分析下时间复杂度，当我们进行increment的时候，我们需要用一个while循环对前`k`个元素进行修改，复杂度是$O(k)$。

&emsp;&emsp;上面这个方法看上去不错，但是我们有没有办法能把所有的操作的时间复杂度都做到$O(1)$呢？这里需要借助一个design类题目常用的思路——lazy operation！对于这道题来说，只有当`pop`的时候我才需要知道某个元素的值，因此在`inc`的时候我其实并不需要更新元素，我只需要记录一些信息，然后在`pop`的时候借助这些信息计算出实际返回的值就可以了。

&emsp;&emsp;由于题目给出了数量的最大值，而且还是小于1000，因此我们放心地开一个数组`inc`，其中`inc[i]`表示从栈底到第`i`个位置的increment值。当进行`inc`操作时，因为对底部的`k`个元素操作，所以`i = k - 1`，这里还需要注意`k`和栈的size的比较，两者之间取较小值，所以综合来说应该是`i = min(k, st.size()) - 1`。然后我们记录`inc[i] += val`，这就表示了底部的`i`个元素需要增加的值。当我们`pop()`获取元素的值时，计算出`i = st.size() - 1`，然后到`inc[i]`中找到需要增加的值加到元素本身就可以了。这里还要注意，因为我们需要对底部剩下的元素生效，所以应该有一个累加的操作，即`inc[i - 1] += inc[i]`，这样就把增加的值传递到之前的元素了。

## Solution

&emsp;&emsp;无。

------

## Code

方法1:

```c++
class CustomStack {
public:
    CustomStack(int maxSize) {
        cap = maxSize;
    }
    
    void push(int x) {
        if (arr.size() < cap) {
            arr.push_back(x);
        }
    }
    
    int pop() {
        if (arr.size() == 0) {
            return -1;
        }
        int result = arr[arr.size() - 1];
        arr.pop_back();
        return result;
    }
    
    void increment(int k, int val) {
        int size = arr.size();
        int num = min(k, size);
        int i = 0;
        while (num--) {
            arr[i++] += val;
        }
    }
private:
    int cap;
    vector<int> arr;
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```

方法2:

```c++
class CustomStack {
public:
    CustomStack(int maxSize) {
        cap = maxSize;
        inc = vector<int>(cap, 0);
    }
    
    void push(int x) {
        if (st.size() < cap) {
            st.push(x);
        }
    }
    
    int pop() {
        if (st.size() == 0) {
            return -1;
        }
        
        int i = st.size() - 1;
        if (i > 0) {
            inc[i - 1] += inc[i];
        }
        int result = st.top() + inc[i];
        inc[i] = 0;
        st.pop();
        return result;
    }
    
    void increment(int k, int val) {
        int i = min(k, (int)st.size()) - 1;
        if (i >= 0) {
            inc[i] += val;            
        }
    }
private:
    int cap;
    stack<int> st;
    vector<int> inc;
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```

------

## Summary

&emsp;&emsp;这道题目很容易想到用方法1解决，也能通过OJ，但是更好的处理应该是使用方法2。方法2的懒加载体现了design类型题目的特点，通过记录变更信息，最后再计算实际结果。这道题目的分享到这里，感谢你的支持！
