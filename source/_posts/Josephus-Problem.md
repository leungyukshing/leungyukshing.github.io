---
title: Josephus Problem(约瑟夫环问题)
tags:
mathjax: true
date: 2020-12-10 11:58:13

---

## Problem

n个人围成一圈，第一个人从0开始报数，报到`(m - 1)`的退出，剩下的人继续从0开始报数。最后剩下的人为胜者。指定`n`和`m`，求胜者编号。

<!-- more -->

**Example 1:**

```
Input:10 2
Output:5
```

------

## Analysis

&emsp;&emsp;这道题目的数学背景是非常著名的约瑟夫环问题（Josephus Problem），问题在上面已经给出，现在我们就来看一下如何求解。

&emsp;&emsp;先来看第一种非常简单直接的解法，用链表实现。首先我们构建一个环，然后按照题目的步骤开始报数，需要退出的时候把当前节点从链表中删除，最后这个链表就只会剩下一个人，这个就是答案。这种方法最大的问题就是时间复杂度是$O(nm)$，因为实际上求解的过程就是模拟整个游戏进行的过程。

&emsp;&emsp;然后我们来看一下在数学方面如何优化计算呢？如果把这个环摊平（每一轮报数都是一个数组），我们观察赢家的位置，看看它是如何变化的。假设`n = 11`，`m = 3`：

![picture](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwNjA0MDAxNzE0MzQ1?x-oss-process=image/format,png)

&emsp;&emsp;通过上图我们可以看到，最后的赢家是7，看它的位置变化。白色的前三行：

1. 第一轮的时候，编号为`3`的人报数2，踢掉；这个时候就从编号为`4`的人开始报数；所以`7`在数组的位置往前移动了`m - 1`位（前面报过数但没有被踢的人有`m - 2`人，还有被踢掉的那个人）;
2. 第二轮的时候，下标为`6`的人报数2，踢掉；这个时候就从编号为`7`的人开始报数；所以`7`在数组的位置往前移动了`m - 1`位；
3. 第三轮的时候，下标为`9`的人报数2，踢掉；这个时候编号为`7`的人已经报过数了，它需要去到数组的后面，所以往前移动`m-1`位（从尾巴开始，所以下标为 `6`）

&emsp;&emsp;可以看到上面这个过程，从开始到结束，赢者的下标一直是在往前移动的；所以从另一个方向看，**因为结束状态的时候，赢者的下标必定是0，所以可以倒推回去，也就是每一步都往后移动**。这里给出数学递推公式：
$$
f(n, m) = (f(n - 1, m) + m) \% presentLength
$$
&emsp;&emsp;这里有一个mod是为了兼容头尾的edge case。

------

## Code

```c++
#include <iostream>
#include <assert.h>
using namespace std;

int josephus_circle(int n, int m) {
	int p = 0;
	for (int i = 2; i <= n; i++) {
		p = (p + m) % i;
	}
	return p + 1;
}
int main() {
	int n, m;
	cin >> n >> m;
	assert(josephus_circle(n, m) == 5);
	return 0;
}
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的题目，用链表实现并不复杂，但是并不是最优的解法。通过了解背后的数学原理，我们有了更加快捷简便的方法。这道题目的分享到这里，谢谢您的支持！

# Reference

1. [约瑟夫环——公式法（递推公式）](
