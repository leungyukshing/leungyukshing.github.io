---
title: LeetCode解题报告（420）-- 489. Robot Room Cleaner
mathjax: true
date: 2021-12-12 21:28:03
---

## Problem

You are controlling a robot that is located somewhere in a room. The room is modeled as an `m x n` binary grid where `0` represents a wall and `1` represents an empty slot.

<!-- more -->

The robot starts at an unknown location in the root that is guaranteed to be empty, and you do not have access to the grid, but you can move the robot using the given API `Robot`.

You are tasked to use the robot to clean the entire room (i.e., clean every empty cell in the room). The robot with the four given APIs can move forward, turn left, or turn right. Each turn is `90` degrees.

When the robot tries to move into a wall cell, its bumper sensor detects the obstacle, and it stays on the current cell.

Design an algorithm to clean the entire room using the following APIs:

```
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
```

**Note** that the initial direction of the robot will be facing up. You can assume all four edges of the grid are all surrounded by a wall.

**Custom testing:**

The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the four mentioned APIs without knowing the room layout and the initial robot's position.

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/07/17/lc-grid.jpg)

```
Input: room = [[1,1,1,1,1,0,1,1],[1,1,1,1,1,0,1,1],[1,0,1,1,1,1,1,1],[0,0,0,1,0,0,0,0],[1,1,1,1,1,1,1,1]], row = 1, col = 3
Output: Robot cleaned all rooms.
Explanation: All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.
```

**Example 2:**

```
Input: room = [[1]], row = 0, col = 0
Output: Robot cleaned all rooms.
```

**Constraints:**

- `m == room.length`
- `n == room[i].length`
- `1 <= m <= 100`
- `1 <= n <= 200`
- `room[i][j]` is either `0` or `1`.
- `0 <= row < m`
- `0 <= col < n`
- `room[row][col] == 1`
- All the empty cells can be visited from the starting position.

------

## Analysis

&emsp;&emsp;这道题目比较新颖，题目只给出一个`robot`对象，然后要求我们完成整幅图的遍历。其实对于二维平面的遍历相信大家都非常熟悉，无非就是BFS和DFS，但是这里的难点在于，我们并不知道地图的样子。所以我说题目的设置非常新颖，但是其本质还是一样的，就是图的遍历。

&emsp;&emsp;先来看看我们有什么，我们可以进行`move`，可以`turnLeft`或`turnRight`。对于遍历来说这三个操作足够了，实际上如果不考虑最少操作的话，`turnLeft`和`turnRight`还可以看作是一个操作。那么我们来看是用BFS还是DFS。两者最大的区别在于，BFS一般是处理最短路径问题，而DFS在处理可达性问题上更加方便。对于机器人行走这种场景来说，显然DFS是更加贴切的。其移动的原则就是：**沿一个方向走，一直走到尽头，然后返回上一个位置，向下一个方向移动**。

&emsp;&emsp;我们的编码实际上就是把上面这句话给实现了。首先我们要判断走到尽头，`robot`类中提供的`move()`函数就能知道是否走到了尽头。怎么返回上一个位置呢？很简单，在当前的位置和方向的情况下，旋转180度，然后走一步，再旋转180度，就是上一个状态了。（特别注意需要两次旋转180度才能回到上一个状态，因为这里上一个状态指的是位置和方向都要相同）。接着来看下一个方向是什么？上面我说到`turnLeft`和`turnRight`实际上是一样的，所以我定义下一个方向就是向右转，当所有的方向处理完之后，也返回上一个状态。

## Solution

&emsp;&emsp;&emsp;这里还要解决一个遍历过程中不走重复路线的问题，返回上一个状态这个回头路是不得不走的，所以这里我们不需要判断是否访问过，但是第一次主动向前走的时候就要判断了。而这道题目特殊的地方就在于我们不知道矩阵的size，因此我使用了`unorderd_map<int, unorderd_set<int>>`来记录访问过的位置。

------

## Code

```c++
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * class Robot {
 *   public:
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     bool move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     void turnLeft();
 *     void turnRight();
 *
 *     // Clean the current cell.
 *     void clean();
 * };
 */

class Solution {
public:
    void cleanRoom(Robot& robot) {
        this->robot = &robot;
        helper(0, 0, 0);
    }
private:
    unordered_map<int, unordered_set<int>> visited;
    Robot* robot;
    
    void helper(int i, int j, int d) {
        int directions[][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
        visited[i].insert(j);
        robot->clean();
        
        for (int k = 0; k < 4; ++k) {
            int newD = (d + k) % 4;
            int newI = i + directions[newD][0];
            int newJ = j + directions[newD][1];
            
            if (!visited[newI].count(newJ) && robot->move()) {
                helper(newI, newJ, newD);
                robot->turnRight();
                robot->turnRight();
                robot->move();
                robot->turnRight();
                robot->turnRight();
            }
            
            robot->turnRight();
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是重新包装过的二维矩阵DFS问题，难点主要在回头路怎么走的问题，其他的部分就是常规套路。这道题目的分享到这里，感谢你的支持！
