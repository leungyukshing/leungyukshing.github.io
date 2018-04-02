---
title: 追赶法
abbrlink: 10592
date: 2018-03-23 10:36:05
tags:
mathjax: true
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

## 追赶法求解矩阵
对于某些特殊的线性方程组，如果还用高斯消元法去求解的话，会有大量的计算，这样是很低效的，在这篇post中，我将给大家讲解一种特殊的求解矩阵的方法--**追赶法**。
<!-- more -->

### 分解推导
对于一个**对角占优的三对角线方程组**：

$$ \begin{bmatrix}
        b_1 & c_1 \\
        a_2 & b_2 & c_2 \\
         & \ddots & \ddots & \ddots & \\
         &  & a_{n-1} & b_{n-1} & c_{n-1}\\
         &  &  & a_n & b_n \\
        \end{bmatrix}
    \begin{bmatrix}
        x_1 \\
        x_2 \\
        \vdots \\
        x_{n-1}\\
        x_n \\
        \end{bmatrix} =
        \begin{bmatrix}
        f_1 \\
        f_2 \\
        \vdots \\
        f_{n-1}\\
        f_n \\
        \end{bmatrix}
$$，
简记为 **A*x* = *f***, 其中，当| *i*-*j* | > 1时， $a_{ij} = 0$， 且：
① | $b_1$ | > | $c_1$ | > 0;
② | $b_i$ | ≥ | $a_i$ | + | $c_i$ |, $a_i$, $c_i$ ≠ 0, *i = 2, 3, ... , n-1*;
③ | $b_n$ | > | $a_n$ | > 0.

根据矩阵**A**的特点，我们对其进行**LU分解**， 即 ***A = LU***。

假设分解后是这样的：
$$ \begin{bmatrix}
        b_1 & c_1 \\
        a_2 & b_2 & c_2 \\
         & \ddots & \ddots & \ddots & \\
         &  & a_{n-1} & b_{n-1} & c_{n-1}\\
         &  &  & a_n & b_n \\
        \end{bmatrix} =
    \begin{bmatrix}
        α_1 \\
        r_2 & α_2 \\
         & \ddots & \ddots \\
         & & r_n & α_n\\
        \end{bmatrix}
        \begin{bmatrix}
        1 & β_1 \\
         & 1 & \ddots \\
         & & \ddots & β_{n-1}\\
         & & & 1\\
        \end{bmatrix}
$$

其中$α_i$, $β_i$, $r_i$为待定系数，通过对比LU分解的结果，可以得出：

$$
\left\{
\begin{aligned}
b_1 &=α_1,& c_1=α_1β_1，\\
a_i &=r_i,& b_i = r_iβ_{i-1} + α_i, & i = 2,3,...,n,\\
c_i & = α_iβ_i,& i = 2, 3,...,n-1
\end{aligned}
\right.
$$
可以推出如下结果：
$$
α_i = b_i - a_iβ_i, i = 2,3,...,n;\\
β_i = c_i / (b_i - a_iβ_i), i = 2,3,...,n-1.
$$

### 追赶法公式
到这里，我们就可以确定了**A**矩阵可以分写成***LU***的形式。然后我们就可以把求解***Ax = f***的问题转化成解两个三角形方程组：
① ***Ly = f***，求***y***; ②***Ux = y***，求***x***。
算法公式如下：
（1）计算{$β_i$}的递推公式
$$
β_i = c_1 / b_1,\\
β_i = c_i / (b_i - a_iβ_{i-1}), i = 2,3,...,n-1;
$$
（2）解***Ly = f***
$$
y_1 = f_1 / b_1,\\
y_i = (f_i - a_iy_{i-1}) / (b_i - a_iβ_{i-1}), i = 2,3,...,n;
$$
（3）解***Ux = y***
$$
x_n = y_n,\\
x_i = y_i - β_ix_{i+1}, i = n-1,n-2,...,2,1.
$$

## 代码实现
```C++
#include <iostream>
#include <iomanip>
using namespace std;
#define maxn 50


int n;
double a[maxn];
double b[maxn];
double c[maxn];
double f[maxn];

double Beta[maxn];
double y[maxn];
double x[maxn];

/*读取矩阵*/
void read() {
  cout << "Please input the scale of matrix n: ";
  cin >> n;
  cout << "|--------------------\n";
  cout << "|Please input the [a]diagonal: " << endl;

  for (int i = 2; i <= n; i++) {
    cin >> a[i];
  }

  cout << "|--------------------\n";
  cout << "|Please input the [b]diagonal: " << endl;
  for (int i = 1; i <= n; i++) {
    cin >> b[i];
  }

  cout << "|--------------------\n";
  cout << "|Please input the [c]diagonal: " << endl;
  for (int i = 1; i <= n - 1; i++) {
    cin >> c[i];
  }

  cout << "|--------------------\n";
  cout << "|Please input the [f]: " << endl;
  for (int i = 1; i <= n; i++) {
    cin >> f[i];
  }
}

/*显示结果*/
void print() {
  cout << "|-------------------" << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << endl;
  }
}

void Chasing() {
  Beta[1] = c[1] / b[1];
  for (int i = 2; i <= n - 1; i++) {
    Beta[i] = c[i] / (b[i] - a[i] * Beta[i - 1]);
  }

  y[1] = f[1] / b[1];
  for (int i = 2; i <= n; i++) {
    y[i] = (f[i] - a[i] * y[i - 1]) / (b[i] - a[i] * Beta[i - 1]);
  }

  x[n] = y[n];
  for (int i = n - 1; i >= 1; i--) {
    x[i] = y[i] - Beta[i] * x[i + 1];
  }

  print();
}

int main() {
  read();
  Chasing();
  return 0;
}
```
值得一提的是，使用**追赶法**求解的**计算复杂度**是***O(n)***，相比起[高斯消元法](https://leungyukshing.github.io/archives/GaussianElimination.html)，追赶法在计算复杂度上有很大的提升。
利用**追赶法**解**对角优三对角矩阵**的算法讲解到这里结束了，谢谢！

---
#### 参考资料：
1.数值分析（第5版）  李庆扬，王能超，易大义 *编*
