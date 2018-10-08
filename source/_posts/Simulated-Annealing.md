---
title: 模拟退火算法
mathjax: true
abbrlink: 24867
date: 2018-10-08 19:59:35
tags:
---

## 算法背景介绍
&emsp;&emsp;模拟退火算法（Simulated Annealing Algorithm）是**元启发式搜索**算法的其中一种。相比起效率低下的随即搜索算法，元启发式搜索算法借助了演化思想和集群智能思想，对解的分布有规律的复杂问题有良好的效果。
<!-- more -->

## 模拟退火算法
&emsp;&emsp;模拟退火法（Simulated Annealing）是克服爬山法缺点的有效方法，所谓**退火**是冶金专家为了达到某些特种晶体结构重复将金属加热或冷却的过程，该过程的控制参数为温度T。模拟退火法的基本思想是，在系统朝着能量减小的趋势这样一个变化过程中，偶尔允许系统跳到能量较高的状态，以避开局部极小点，最终稳定到全局最小点。
&emsp;&emsp;算法的关键问题是：什么时候允许增加能量？应该增加多少能量？

## 基本步骤
&emsp;&emsp;模拟退火算法步骤如下：
  1. 随机挑选出一单元，并给他一个随机的位移$k$，求出系统因此而产生的能量变化$\Delta E_k$。
  2. 若$\Delta E_k \leq0$，该位移可采纳，而变化后的系统状态可作为下次变化的起点；若$\Delta E_k \gt0$，位移后的状态可采纳的概率为
  $$
  P_k = \frac{1}{(1+e^{-\Delta E_k/T})}
  $$
  式中T为温度，然后从(0,1)区间均匀分布的随机数中挑选一个数R。若$R \lt P_k$，则将变化后的状态作为下次的七点；否则，将变化前的状态作为下次的起点。
  3. 转第（1）步继续执行，直至达到平衡状态为止。


## 实用例子
&emsp;&emsp;使用模拟退火法解决[TSP问题](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。代码如下：
```C++
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <cmath>

using namespace std;

#define T0 50000.0 // 初始温度
#define T_end (1e-8) //
#define q 0.98 // 退火系数
#define L 1000 // 每个温度时的迭代次数
#define N 31 // 城市数量
int city_list[N]; // 存放一个解
double city_pos[N][2] = {{1304,2312},{3639,1315},{4177,2244},{3712,1399},{3488,1535},{3326,1556},{3238,1229},{4196,1004},{4312,790},
    {4386,570},{3007,1970},{2562,1756},{2788,1491},{2381,1676},{1332,695},{3715,1678},{3918,2179},{4061,2370},{3780,2212},{3676,2578},{4029,2838},
    {4263,2931},{3429,1908},{3507,2367},{3394,2643},{3439,3201},{2935,3240},{3140,3550},{2545,2357},{2778,2826},{2370,2975}};

// 函数
double distance(double *, double *); // 计算两个城市之间的距离

double path_len(int *); // 计算路径长度

void init();

void create_new(); // 产生新解

double distance(double *city1, double *city2) {
  double x1 = *city1;
  double y1 = *(city1 + 1);
  double x2 = *(city2);
  double y2 = *(city2 + 1);

  double dis = sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2));
  return dis;
}

double path_len(int *arr) {
  double path = 0;
  int index = *arr; // 城市序号
  for (int i = 0; i < N - 1; i++) {
    int index1 = *(arr + i); // 起点
    int index2 = *(arr + 1 + i); // 终点
    double dis = distance(city_pos[index1 - 1], city_pos[index2 - 1]);
    path += dis;
  }
  // 计算回路
  int last_index = *(arr + N - 1); // 最后一个城市序号
  int first_index = *arr; // 第一个城市序号
  double last_dis = distance(city_pos[last_index - 1], city_pos[first_index - 1]);
  path += last_dis;
  return path;
}

void init() {
  for (int i = 0; i < N; i++) {
    city_list[i] = i+1; // 初始化一个解
  }
}

// 随机交叉两个位置的方式产生新的解
void create_new() {
  double r1 = ((double)rand()) / (RAND_MAX + 1.0);
  double r2 = ((double)rand()) / (RAND_MAX + 1.0);
  int pos1 = (int)(N * r1); // 第一个交叉点位置
  int pos2 = (int)(N * r2); // 第二个交叉点位置
  int temp = city_list[pos1];
  city_list[pos1] = city_list[pos2];
  city_list[pos2] = temp;
}

int main() {
  srand((unsigned)time(NULL)); // 初始化随机数种子
  time_t start, finish;
  start = clock(); // 开始计时
  double T;
  int count = 0; // 记录降温次数
  T = T0;
  init();
  int city_list_copy[N]; // 用于保存原始解
  double f1, f2, df; // f1为初始解目标函数值, f2为新解目标函数值,df为二者差值
  double r; // 0-1之间随机数，用来决定是否接受新解

  // 当温度低于结束温度时，退火结束
  while (T > T_end) {
    for (int i = 0; i < L; i++) {
      memcpy(city_list_copy, city_list, N * sizeof(int)); // 复制数组
      create_new(); // 产生新解
      f1 = path_len(city_list_copy);
      f2 = path_len(city_list);
      df = f2 - f1;

      // 退火准则
      if (df >= 0) {
        r = ((double)rand()) / (RAND_MAX);
        // 保留原来的解
        if (exp(-df / T) <= r) {
          memcpy(city_list, city_list_copy, N * sizeof(int));
        }
      }
    }
    T *= q; // 降温
    count++;
  }

  finish = clock(); // 退火结束
  double duration = ((double)(finish - start)) / CLOCKS_PER_SEC;
  cout << "Simulated Annealing: " << endl;
  cout << "Initial Temperature T0 = " << T0 << ", Cooling Coefficient q = " << q << ", Loops per Temperature: " << L << ", Total Cooling: " <<  count << "times" << endl;

  cout << "Best Path: " << endl;
  for (int i = 0; i < N - 1; i++) {
    cout << city_list[i] <<  "-->";
  }
  cout << city_list[N - 1];

  cout << "\nBest Path Length: " << path_len(city_list) << endl;

  cout << "Time for Algorithm: " << duration << "sec" << endl;
  return 0;
}
```

运行结果：
![模拟退火法](/images/SA.png)

## 小结
&emsp;&emsp;理解模拟退火法的核心思想是**以一定概率接受比当前解更差的解**。对于每一个新生成的解，我们都需要有一个概率去接受更差的解。相比起启发式搜索，元启发式搜索更通用，在解决某些问题有一定的优势。希望这篇博客能够帮助到你，谢谢！
