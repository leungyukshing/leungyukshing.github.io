---
title: LeetCode解题报告（219）-- 858. Mirror Reflection
tags:
  - LeetCode
mathjax: true
date: 2020-11-18 01:47:28

---

## Problem

There is a special square room with mirrors on each of the four walls.  Except for the southwest corner, there are receptors on each of the remaining corners, numbered `0`, `1`, and `2`.

The square room has walls of length `p`, and a laser ray from the southwest corner first meets the east wall at a distance `q` from the `0`th receptor.

Return the number of the receptor that the ray meets first.  (It is guaranteed that the ray will meet a receptor eventually.)

<!-- more -->

**Example 1:**

![Example1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/18/reflection.png)

```
Input: p = 2, q = 1
Output: 2
Explanation: The ray meets receptor 2 the first time it gets reflected back to the left wall.
```

**Note:**

1. `1 <= p <= 1000`
2. `0 <= q <= p`

------

## Analysis

&emsp;&emsp;这道题第一眼看下去是计算几何的感觉，牵涉到入射角、出射角好像很麻烦。但是注意到这里的镜面反射是正方形，这里面有一些性质是可以利用的。在一个正方形里面反射，实际上就像是在两排平行的镜子中反射，如下图所示。这里没有很严格的理论证明，自己动手在纸上画一下就知道了。

![](https://s3-lc-upload.s3.amazonaws.com/users/motorix/image_1529877876.png)

&emsp;&emsp;然后我们来看上图这种情况，这束光线一共折射了两次，镜子拓展了一次。同时我们找到了一条有关`p`和`q`的关系：$2p = 3q$。`p`、`q`前面的参数实际上和光线的折射次数和镜子拓展的次数是相关的，假设$mp = nq$，则有：

- `m `：镜子拓展次数 + 1；
- `n`：光线折射次数 + 1；

&emsp;&emsp;由这条等式可以知道，`m`、`n`不可能同时为偶数，因为如果都是偶数的话，就可以对等式进行化简，说明在之前这束光线就已经被其中一个接收器接收了。然后我们分类讨论：

- `n`为偶数时，说明光的折射次数是奇数，又因为这束光线是从左边射出的，奇数次折射后仍然折射到左边，所以只能是2号接收器接收；
- `n`为奇数时，说明光的折射次数是偶数，经过偶数次折射后这束光线就折射到右手边，所以可能是0或1号接收器。
  - 如果房子扩展了奇数次（例如一次），此时`m`为偶数，则这束光线一定是到1号接收器；
  - 如果`m`为奇数，则这束光线就会到0号接收器。

------

## Solution

&emsp;&emsp;按照上面的分析，分别求出`m`和`n`，然后根据奇偶性判断结果即可。

------

## Code

```c++
class Solution {
public:
    int mirrorReflection(int p, int q) {
        int m = q, n = p;
        // mq = np
        while (m % 2 == 0 && n % 2 == 0) {
            m /= 2;
            n /= 2;
        }
        
        if (m % 2 == 0 && n % 2 == 1) {
            return 0;
        } else if (m % 2 == 1 && n % 2 == 1) {
            return 1;
        } else if (m % 2 == 1 && n % 2 == 0) {
            return 2;
        }
        return 0;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目涉及到了一些物理的知识，虽然不需要计算反射的过程，但是要利用反射相关的知识去找到规律，充分利用正方形内镜面反射的特点，找到反射次数和镜子拓展的规律。这道题目的分享到这里，谢谢！

------

## Reference

1. [【LeetCode】858. Mirror Reflection 解题报告（Python）](
