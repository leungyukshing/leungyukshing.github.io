---
title: LeetCode解题报告（503）-- 362. Design Hit Counter
mathjax: true
date: 2022-01-10 21:44:32
---

## Problem

Design a hit counter which counts the number of hits received in the past `5` minutes (i.e., the past `300` seconds).

Your system should accept a `timestamp` parameter (**in seconds** granularity), and you may assume that calls are being made to the system in chronological order (i.e., `timestamp` is monotonically increasing). Several hits may arrive roughly at the same time.

Implement the `HitCounter` class:

- `HitCounter()` Initializes the object of the hit counter system.
- `void hit(int timestamp)` Records a hit that happened at `timestamp` (**in seconds**). Several hits may happen at the same `timestamp`.
- `int getHits(int timestamp)` Returns the number of hits in the past 5 minutes from `timestamp` (i.e., the past `300` seconds).

<!-- more -->

**Example 1:**

```
Input
["HitCounter", "hit", "hit", "hit", "getHits", "hit", "getHits", "getHits"]
[[], [1], [2], [3], [4], [300], [300], [301]]
Output
[null, null, null, null, 3, null, 4, 3]

Explanation
HitCounter hitCounter = new HitCounter();
hitCounter.hit(1);       // hit at timestamp 1.
hitCounter.hit(2);       // hit at timestamp 2.
hitCounter.hit(3);       // hit at timestamp 3.
hitCounter.getHits(4);   // get hits at timestamp 4, return 3.
hitCounter.hit(300);     // hit at timestamp 300.
hitCounter.getHits(300); // get hits at timestamp 300, return 4.
hitCounter.getHits(301); // get hits at timestamp 301, return 3.
```

**Example 2:**

```
Input: bottom = "AAAA", allowed = ["AAB","AAC","BCD","BBE","DEF"]
Output: false
Explanation: The allowed triangular patterns are shown on the right.
Starting from the bottom (level 4), there are multiple ways to build level 3, but trying all the possibilites, you will get always stuck before building level 1.
```

**Constraints:**

- `1 <= timestamp <= 2 * 109`
- All the calls are being made to the system in chronological order (i.e., `timestamp` is monotonically increasing).
- At most `300` calls will be made to `hit` and `getHits`.

 

**Follow up:** What if the number of hits per second could be huge? Does your design scale?

---

## Analysis

&emsp;&emsp;这是一道设计类题目，是一个基于时间的计数器，每次`hit`会有一个时间戳，然后有一个`getHits`的方法返回过去300 seconds内一共有多少个hit。因为`hit`是基于时间顺序的，这意味着传入的timestamp是递增的，这种先进先出的顺序就很适合使用队列作为数据结构。所以这就有了一个初步的实现，以队列为基本的数据结构，每次`hit`就把timestamp放进队列，在`getHits()`的时候维护一个300 seconds的区间，只需要检查队头的元素是否在区间中即可，最后返回队列的size就是在这个区间中hit的数量。

&emsp;&emsp;然后题目给了一个follow up，说如果每秒的hit数量很多怎么办。如果按照上面实现的方法，比如在`timestamp = 20`的时候有100个call，那么队列中就会有100个元素，首先占据的空间就比较多。其次，当我在`timestamps = 400`的时候`getHits()`，我至少要把这100个值为20的元素从队列中`pop`出去，这里也是很耗时间，所以我们就整合压缩一下，相同时间戳的就采用计数的方式压缩成一个`pair`，这样100个相同时间的hit实际上在队列中只占1个位置。这样处理之后，为了快速得到hit的个数，我们再用一个`count`变量去维护总数即可，因为此时的队列size已经不是hit的数量了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class HitCounter {
public:
    HitCounter() {
        count = 0;
    }
    
    void hit(int timestamp) {
        if (q.empty() || timestamp != q.back().first) {
            q.push({timestamp, 1});
        } else {
            ++q.back().second;
        }
        ++count;
    }
    
    int getHits(int timestamp) {
        while (!q.empty() && (timestamp - q.front().first) >= 300) {
            count -= q.front().second;
            q.pop();
        }
        return count;
    }
private:
    queue<pair<int, int>> q; // <time, counts>
    int count;
};

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter* obj = new HitCounter();
 * obj->hit(timestamp);
 * int param_2 = obj->getHits(timestamp);
 */
```

------

## Summary

&emsp;&emsp;这道题目属于比较简单的设计题，首先基于时间先进先出的特点确定使用队列，这个指向非常明显，而follow up的处理也很符合日常的逻辑，采用计数的方法。这道题目的分享到这里，感谢你的支持！

