---
title: LeetCode解题报告（479）-- 1040. Moving Stones Until Consecutive II
mathjax: true
date: 2021-12-31 23:29:14
---

## Problem

There are some stones in different positions on the X-axis. You are given an integer array `stones`, the positions of the stones.

Call a stone an **endpoint stone** if it has the smallest or largest position. In one move, you pick up an **endpoint stone** and move it to an unoccupied position so that it is no longer an **endpoint stone**.

- In particular, if the stones are at say, `stones = [1,2,5]`, you cannot move the endpoint stone at position `5`, since moving it to any position (such as `0`, or `3`) will still keep that stone as an endpoint stone.

The game ends when you cannot make any more moves (i.e., the stones are in three consecutive positions).

Return *an integer array* `answer` *of length* `2` *where*:

- `answer[0]` *is the minimum number of moves you can play, and*
- `answer[1]` *is the maximum number of moves you can play*.

<!-- more -->

**Example 1:**

```
Input: stones = [7,4,9]
Output: [1,2]
Explanation: We can move 4 -> 8 for one move to finish the game.
Or, we can move 9 -> 5, 4 -> 6 for two moves to finish the game.
```

**Example 2:**

```
Input: stones = [6,5,4,3,10]
Output: [2,3]
Explanation: We can move 3 -> 8 then 10 -> 7 to finish the game.
Or, we can move 3 -> 7, 4 -> 8, 5 -> 9 to finish the game.
Notice we cannot move 10 -> 2 to finish the game, because that would be an illegal move.
```



**Constraints:**

- `3 <= stones.length <= 104`
- `1 <= stones[i] <= 109`
- All the values of `stones` are **unique**.

---

## Analysis

&emsp;&emsp;题目给出一个`stones`数组，里面的值代表石头放置的位置。定义处于两边的石头叫做`endpoint`，每次移动只能把一个`endpoint`的石头移动到非`endpoint`的位置，问最大和最小的移动次数。

&emsp;&emsp;首先这是两个问题，只不过要放在一个vector中返回，所以我们分别来看这两个问题。先来看最小的移动次数，最小的移动次数就和之前最少移动多少个1使得01数组中的1都堆在一起的题目很类似，基本的思路就是使用sliding window，统计每个区间中已有的数量（在这道题目就是石头的数量），然后就能计算出空缺的位置，维护一个空缺位置的极小值就是答案。这里有一个edge case需要注意，比如`[3,4,5,6,10]`的时候，虽然我们滑动窗口扫到前四个是连在一起的，空缺的位置是1，但并不是1个move就可以完成，因为不能把10直接移动到7（endpoint位置必须移动到非endpoint位置）。所以要判断如果前面是连续的话，说明没有空间塞了，因此要2个move才能完成（先把3移动到8，再把10移动到7）。

&emsp;&emsp;解决完最小值问题，再来看最大值问题。最大值实际上是一个贪心的算法，我们只需要把元素都往左边靠，或者都往右边靠就是最大的moves。虽然没有严格的证明，但是仍然可以在直观上有一个判断。如果石头一开始都堆在左边，那么把石头都往右边挪，这样move就是最大的；如果石头一开始都堆在右边，那么把石头都往左挪，move就是最大。然后我们来看怎么计算这个最大的moves。先假设我们从左边把石头都挪到最右边，所以第一次我们把`stones[0]`移动到`stones[1] + 1`的位置，试想一下从`stones[1]`到`stones[n - 1]`一共有`x`个空位，那么最大的move将会是逐个把这些空位填满，每次都是用最左边的那个石头去填，所以我们只需要计算出来`stones[1]`到`stones[n - 1]`之间有多少个空位。为什么不是`stones[0]`到`stones[n - 1]`？还是那个问题，endpoint石头不能移动到endpoint位置。因此它们直接有`stones[n - 1] - stones[1] + 1`个位置，其中有`n - 1`个石头，所以一共有空位是`stones[n - 1] - stones[1] + 1 - (n - 1) = stones[n - 1] - stones[1] - n + 2`。

&emsp;&emsp;同理，把石头从右边挪到左边也是一样的，一共需要的moves是`stones[n - 2] - stones[0] - n + 2`，所以最大的moves就是在两者之间取个较大值即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> numMovesStonesII(vector<int>& stones) {
        sort(stones.begin(), stones.end());
        
        int n = stones.size();
        int low = n;
        int high = max(stones[n - 1] - stones[1] - n + 2, stones[n - 2] - stones[0] - n + 2);
        int i = 0;
        for (int right = 0, left = 0; right < n; ++right) {
            while (stones[right] - stones[left] + 1 > n) {
                ++left;
            }
            //cout << i << " " << j << endl;
            int occupied = right - left + 1;
            
            if (occupied == n - 1 && stones[right] - stones[left] + 1 == n - 1) {
                low = min(low, 2);
            } else {
                low = min(low, n - occupied);
            }
        }
        return {low, high};
    }
};
```

------

## Summary

&emsp;&emsp;这道题目综合了sliding window和贪心算法两个知识点，是非常有意思的题目。这里特别想总结下最小moves，sliding window通常是这类题目的解法，通过维护指定的长度，计算出空缺的位置，即是需要move的数量。这道题目的分享到这里，感谢你的支持！

