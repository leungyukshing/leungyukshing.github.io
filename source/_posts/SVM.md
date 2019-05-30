---
title: 支持向量机--SVM
tags:
  - Machine Learning
abbrlink: 59402
date: 2019-03-22 20:48:22
mathjax: true
---

## 前言

&emsp;&emsp;支持向量机，Support Vector Machine（SVM）是机器学习中处理分类问题一个很重要的方法。SVM最有价值的地方在于它给我们提供了一个解决问题的思路，本篇博客我们就一起来看SVM到底是怎么去解决一个分类问题的。（内容会很长，希望大家能够耐心看完）。
<!-- more -->

---

## 什么是SVM

&emsp;&emsp;当我们面对一个二分类问题的时候，假设数据是**线性可分的**，以二维平面为例，也就是说，我们能够用一条直线来将数据分开为两部分。对于高维问题而言，这条**直线**我们成为**超平面**。这条直线的位置和两种类别的数据点分布的位置是相关的，两类数据点之间是有**间隔**的，我们最优化的过程就是去寻找这个最大的间隔。

<img src="/images/svm1.png" width="400px">

<img src="/images/svm2.png" width="400px">

&emsp;&emsp;如图，对于同一个数据集，我们能够有很多种方法去找到一条直线去分隔开两类数据，而第一种不是我们最想要的是，第二种才是。为什么呢？我们可以通过对原始数据随机增加噪声的方法，去测试这个模型的**推广能力**。你会发现，如果两类数据之间的间隔特别小的话，随机增加噪声后，许多数据点就会被错误地分类；而如果间隔足够大的话，增加噪声后错误分类的个数明显变少。这也说明，SVM在构建模型的时候，已经将其推广能力考虑在内，甚至可以说，SVM是基于模型推广能力而构建起的一个分类模型。与之对比的是，逻辑回归是在后续操作中，通过正则项来增强模型推广能力。

&emsp;&emsp;所以我们就可以大致总结一下SVM是在干什么。SVM就是要寻找一个用于分隔开两类数据的超平面，而这个**最优解就是那个具有最大间隔的超平面**。其中，位于这个最优解两侧的虚线所穿过的样本点，就叫做**支持向量（Support Vector）**。

<img src="/images/svm3.png" width="400px">

---

## 1.建立数学模型

&emsp;&emsp;前面说到我们的最优化问题为我们期望的是分类间隔的最大化。首先我们来对**决策面**、**分类间隔**来做数学描述，然后确定这个优化问题的**约束条件**，最后给出求解的方法。

### 决策面方程

&emsp;&emsp;这里所说的决策面就是用于分隔两类数据的超平面，在二维空间中，我们的表示为：
$$
y=ax+b
$$
&emsp;&emsp;为了贴合机器学习中输入的特征值，将$x$替换为$x_1$，将$y$替换成$x_2$：
$$
x_2=ax_1+b \\
ax_1-x_2+b=0
$$
&emsp;&emsp;写成向量化的形式：
$$
\begin{bmatrix}
{a} & {-1}
\end{bmatrix}
\begin{bmatrix}
{x_1} \\
{x_2}
\end{bmatrix}+ b = 0
$$
&emsp;&emsp;用$w$表示权重，$x$表示特征，$\gamma$表示阈值，这三者都是向量，可以写成：
$$
w^Tx+\gamma=0
$$
&emsp;&emsp;其中，$w=\begin{bmatrix}{w_1, w_2}\end{bmatrix}^T$，$x=\begin{bmatrix}{x_1, x_2}\end{bmatrix}^T$。这两个是具有特殊的几何意义的，$w$是直线的**法向量**，$\gamma$决定了直线的**截距**。

&emsp;&emsp;于是，我们就可以将这条公式推广到$n$维空间中了，也就是说，一个超平面方程必然能写成：
$$
w^Tx+\gamma=0
$$
&emsp;&emsp;其中，$w=\begin{bmatrix}{w_1, w_2, \cdots,w_n}\end{bmatrix}^T$，$x=\begin{bmatrix}{x_1, x_2, \cdots, x_n}\end{bmatrix}^T$。

### 分类间隔方程

&emsp;&emsp;同样，我们以二维空间为例进行推导。在二维空间中，点到直线的距离公式为：
$$
d=\left|\frac{Ax_0+By_0+C}{\sqrt{A^2+B^2}}\right|
$$
&emsp;&emsp;其中，直线方程为$Ax_0+By_0+C=0$，点坐标为$(x_0, y_0)$。

&emsp;&emsp;推广到高维空间，我们上面已经求得超平面方程，因此距离为：
$$
d=\frac{\left|w^Tx+\gamma\right|}{\parallel w \parallel}
$$
&emsp;&emsp;这个$d$就是分类间隔。他表示某个数据点到决策面的距离。因此这就给出了我们去衡量一个决策面好坏的评判方法——**根据分类间隔的大小来判断分类器的好坏，间隔越大，我们认为该超平面分类的效果越好**。于是，问题从求解一个超平面转化为**最大化分类间隔**的问题。

### 约束条件

&emsp;&emsp;上面我们一直讨论的是如何表达**决策面**和**分类间隔**，但是最关键的分类问题还是没有提到。在SVM下，分类问题是作为约束条件出现的。

&emsp;&emsp;首先对于数据集，有两种点：

+ 红色点为正样本，标记为+1
+ 蓝色点为负样本，标记为-1

&emsp;&emsp;为什么要标记成+1和-1（而不是0和1）？这在后面会解释。将语言表示为数学形式：
$$
y_i = \begin{cases}
+1 \quad \quad红色点\\
-1 \quad \quad蓝色点\\
\end{cases}
$$
&emsp;&emsp;如果我们求出的超平面能够对所有数据点进行正确分类，就会满足：
$$
\begin{cases}
w^Tx_i+\gamma\gt0 \quad y_i=1\\
w^Tx_i+\gamma\lt0 \quad y_i=-1\\
\end{cases}
$$
&emsp;&emsp;我们再提高要求，要求这个超平面位于间隔区域的中间，对应的支持向量到超平面的距离为$d$，那么有：
$$
\begin{cases}
\frac{w^Tx_i+\gamma}{\parallel w\parallel} \ge d \quad \forall y_i=1\\
\frac{w^Tx_i+\gamma}{\parallel w\parallel} \le -d \quad \forall y_i=-1\\
\end{cases}
$$
&emsp;&emsp;上述的公式的几何意义是，对于任意样本，他们到决策面的距离（绝对值）都大于$d$。同时除以$d$，得到：
$$
\begin{cases}
\frac{w_d^Tx_i+\gamma_d}{\parallel w\parallel} \ge 1 \quad \forall y_i=1\\
\frac{w_d^Tx_i+\gamma_d}{\parallel w\parallel} \le -1 \quad \forall y_i=-1\\
\end{cases}
$$
&emsp;&emsp;其中$w_d=\frac{w}{\parallel w\parallel d}$，$\gamma_d=\frac{\gamma}{\parallel w\parallel d}$。可以看出，$w_d$和$\gamma_d$依然是两个矢量，他们同样是在空间中描述一个超平面。所以我们可以直接表示为：（只是符号改变了，意义并没有改变）：
$$
\begin{cases}
w^Tx_i+\gamma \ge 1 \quad \forall y_i=1\\
w^Tx_i+\gamma \le -1 \quad \forall y_i=-1\\
\end{cases}
$$
&emsp;&emsp;以上就是SVM最优化问题的约束条件。这里就回答之前提到的，为什么要标记成+1和-1。我们可以发现，这样标记的好处就是可以将这个约束条件写成统一形式：
$$
y_i(w^Tx_i+\gamma) \ge 1 \quad \forall x_i
$$

### SVM优化问题概述

&emsp;&emsp;现在我们整理一下思路，我们需要最优化的目标是分类间隔：
$$
d=\frac{\left|w^Tx+\gamma\right|}{\parallel w \parallel}
$$
&emsp;&emsp;因为我们只用支持向量上的样本点进行求解，这些都满足：
$$
\left | w^Tx_i+\gamma\right| = 1 \quad \forall支持向量上的样本点x_i
$$
&emsp;&emsp;所以待优化的目标函数可以简化成：
$$
d=\frac{1}{\parallel w\parallel}
$$
&emsp;&emsp;为了求解过程中求导的方便，我们将极大化问题转化为极小化问题，并添加上约束条件，得到SVM的优化问题：
$$
min\frac12\parallel w \parallel^2 \\
s.t.\quad y_i(w^Tx_i+b) \ge 1,i=1,2,\cdots,n
$$

### 求解方法

&emsp;&emsp;我们讨论的是[凸优化问题](https://blog.csdn.net/lzldhhaha/article/details/76382962)的求解。大致有如下几类：

+ **无约束**优化问题：
  $$
  minf(x)
  $$
  求解方法：求导，在极值处求得最优值。

+ **有等式约束**的优化问题：
  $$
  minf(x) \\
  s.t. \quad h_{i}(x)=0, \quad i=1,2,\cdots,n \\
  $$
  求解方法：拉格朗日乘子法（Lagrange Multiplier）。

+ **有不等式约束**的优化问题：
  $$
  minf(x) \\
  s.t. \quad g_{i}(x) \le 0, \quad i=1,2,\cdots,n \\
  h_j(x)=0, \quad j=1,2,\cdots,m
  $$
  求解方法：拉格朗日乘子法加上KKT条件。

&emsp;&emsp;很明显，我们的SVM优化问题是有不等式约束的优化问题，因此我们要使用第三种方法。我们先来看看什么是**拉格朗日函数**和**KKT条件**。

#### 拉格朗日函数

&emsp;&emsp;拉格朗日函数出现的思路是，我们当前的最小化问题很难求解，所以我想构造一个函数，这个新的函数在可行解区域内与原函数完全一致，而在可行解区域之外的值非常大（因为非常大的话就不会影响极小值的问题）。那么这个**新的无约束条件的函数的优化问题**就等价于**原来有约束条件的优化问题**。它的核心思路是将有约束转化为无约束。

&emsp;&emsp;对偶的意思是，对于一个优化问题，总有另一个优化问题与之对应。使用对偶的目的是求解拉格朗日函数。

&emsp;&emsp;我们先将原有的函数转化为新构造的拉格朗日函数：
$$
\mathcal{L}(w,b,\alpha)=\frac12\parallel w\parallel^2-\sum^n_{i=1}\alpha_i(y_i(w^Tx_i+b)-1)
$$
&emsp;&emsp;其中$\alpha_i$是拉格朗日乘子，$\alpha_i \ge 0$。令：
$$
\theta(w)= \max_{\alpha_i \ge 0}\mathcal{L}(w,b,\alpha)
$$
&emsp;&emsp;当样本不满足约束的时候，我们就认为它在**可行解区域外**：
$$
y_i(w^Tx_i+b) \lt1
$$
&emsp;&emsp;此时我们要将$\alpha_i$设置为$\infty$，也就是说$\theta(w)$也是$\infty$。

&emsp;&emsp;所以我们新的目标函数为：
$$
\theta(w) = \begin{cases}
\frac12\parallel w \parallel^2 \quad \quad x \in可行解区域\\
+\infty \quad \quad x\in非可行解区域\\
\end{cases}
$$
&emsp;&emsp;于是我们的问题变成了：
$$
\min_{w,b}\theta(w)=\min_{w,b}\max_{\alpha_i\ge0}\mathcal{L}(W,b,\alpha) = p^\*
$$
&emsp;$p\*$表示问题的最优解，和最初的问题是等价的。

&emsp;&emsp;但是我们发现，上面这个问题是不好求解的。原因是，我们需要在带有参数$w,b​$的情况下，对参数$\alpha_i​$先求一个最大值，同时要要兼顾$\alpha_i​$的约束，然后再求最小值。我们使用前面提到的对偶性，将问题转化为：
$$
\max_{\alpha_i \ge 0}\min_{w,b}\mathcal{L}(w,b,\alpha) = d^\*
$$
&emsp;&emsp;$d^\*$表示新的对偶问题的最优值，而且$d^\* \le p^\*$。我们最关心的就是这个等号什么时候能够成立。条件有二：

+ 这个优化问题是凸优化问题。
+ 满足KKT条件。

&emsp;&emsp;凸优化问题的定义是：求最小值的目标函数为凸函数的一类优化问题。我们优化的函数是凸函数，同时求的是极小值，所以这个问题是凸优化问题。我们接下来要关注KKT条件。

#### KKT条件

&emsp;&emsp;KKT条件给出的是最优值必须满足的条件，有三条：

+ 条件1：经过拉格朗日函数处理之后的新目标函数$\mathcal{L}(w,b,\alpha)$对$\alpha$求导为0；
+ 条件2：$h(x)=0$;
+ 条件3：$\alpha * g(x)=0$

### 对偶问题求解

&emsp;&emsp;我们先求解内层的极小化问题。
$$
\frac{\partial\mathcal{L}}{\partial w}=0 \Rightarrow w=\sum^n_{i=1}\alpha_iy_ix_i \\
\frac{\partial\mathcal{L}}{\partial b}=0 \Rightarrow \sum^n_{i=1}\alpha_iy_i=0
$$
&emsp;&emsp;将上述结果代回$\mathcal{L}(w, b, \alpha)$中，得：
$$
\begin{aligned}
\mathcal{L}(w,b,\alpha)&=\frac12\parallel w \parallel^2-\sum^n_{i=1}\alpha_i[y_i(w^Tx_i+b)-1] \\
&= \frac12w^Tw-w^T\sum^n_{i=1}\alpha_iy_ix_i-b\sum^n_{i=1}\alpha_iy_i+\sum^n_{i=1}\alpha_i \\
&= \frac12w^T\sum^n_{i=1}\alpha_iy_ix_i-w^T\sum^n_{i=1}\alpha_iy_ix_i-b\cdot0+\sum^n_{i=1}\alpha_i \\
&= \sum^n_{i=1}\alpha_i-\frac12(\sum^n_{i=1}\alpha_iy_ix_i)^T\sum^n_{i=1}\alpha_iy_ix_i \\
&= \sum^n_{i=1}\alpha_i-\frac12\alpha_i\alpha_jy_iy_jx_i^Tx_j
\end{aligned}
$$
&emsp;&emsp;内侧的min求解完后，接下来求解外侧的max：
$$
\max_\alpha\sum^n_{i=1}\alpha_i-\frac12\sum^n_{i,j=1}\alpha_i\alpha_jy_iy_jx_i^Tx_j \\
s.t. \quad \alpha_i \ge0,i=1,2,\cdots,n \\
\sum^n_{i=1}\alpha_iy_i=0
$$
&emsp;&emsp;至此，我们就将SVM复杂的优化问题转化为上面这个优化问题。可能你会疑问，上面这个问题同样地复杂，我们需要怎么求解呢？SMO算法为我们提供了很好的方法。

---

## 2.SMO算法

&emsp;&emsp;SMO算法（Sequential Minimal Optimization）是Platt发布的一个算法，它的思路是将大优化问题分解成为多个小的优化问题来求解。这些小的优化问题一般比较容易求解，并且它们顺序求解的结果与整体求解的结果是一样的。SMO算法的papaer链接在最后给出。

&emsp;&emsp;SMO算法是通过求取一系列的$\alpha$和$b$，最后再求出$w$，这样我们就得出了决策面。

&emsp;&emsp;SMO算法的工作原理是每次循环选择两个$\alpha$进行优化。为什么是两个呢？因为对$\alpha$我们有约束，一个$\alpha$的增大必然导致另一个$\alpha$的减小，因此只调整一个$\alpha$是不可行的。其中$\alpha$必须满足两个条件：

+ 两个$\alpha$必须要在间隔边界之外。
+ 两个$\alpha$还没有进行过区间化处理或者不在边界上。

&emsp;&emsp;输出函数为：
$$
u=w^Tx+b
$$
&emsp;&emsp;替换$w$，得到：
$$
u=\sum^n_{i=1}\alpha_iy_ix^T_ix+b
$$
&emsp;&emsp;将目标函数变形（最大值问题转化为最小值问题）：
$$
\min_\alpha\frac12\sum^n_{i=1}\sum^n_{j=1}\alpha_i\alpha_jy_iy_jx_i^Tx_j-\sum^n_{i=1}\alpha_i \\
s.t. \quad \alpha_i \ge0,i=1,2,\cdots,n \\
\sum^n_{i=1}\alpha_iy_i=0
$$
&emsp;&emsp;考虑到我们需要分类的数据不一定是100%线性可分的，我们的约束条件需要做出适当地调整。通过引入**松弛变量**，允许少量数据点可以处于超平面错误的一侧。调整后的约束条件为：
$$
s.t. \quad C \ge \alpha_i \ge 0, i=1,2,\cdots,n \\
\sum^n_{i=1}\alpha_iy_i=0
$$
&emsp;&emsp;其中$\alpha_i$取值的意义为：
$$
\begin{aligned}
\alpha_i=0 &\Leftrightarrow y_iu_i \ge1 \\
0 \lt \alpha_i \lt C &\Leftrightarrow y_iu_i = 1 \\
\alpha_i=C &\Leftrightarrow y_iu_i \le 1
\end{aligned}
$$

+ 第一种情况：$\alpha_i$正确分类，在边界内部；
+ 第二种情况：$\alpha_i$是支持向量，在边界上；
+ 第三种情况：$\alpha_i$在两条边界之间。

&emsp;&emsp;同时还有第二个约束：
$$
\sum^n_{i=1}\alpha_iy_i=0
$$
&emsp;&emsp;前面也提到，因为这个约束，我们需要成对更新$\alpha$，不失一般性地，我们选择两个乘子$\alpha_1$和$\alpha_2$：
$$
\alpha_1^{new}y_1+\alpha_2^{new}y_2=\alpha_1^{old}y_1+\alpha_2^{old}y_2 = k
$$
&emsp;&emsp;其中$k$是常数。我们很难同时求解两个因子，因此可以先固定一个，求出一个，再求另一个。这里的思路是先求$\alpha_2^{new}$，再求$\alpha_1^{new}$。我们需要确定$\alpha_2^{new}$的取值范围，假设它的上下界分别为$H$和$L$，那么有：
$$
L \le \alpha_2^{new} \le H
$$

### 推导$\alpha$

&emsp;&emsp;接下来分两种情况讨论：

+ 当$y_1 \neq y_2$，一个为1，一个为-1，得：
  $$
  \alpha_1^{old}-\alpha_2^{old}=k
  $$
  所以$L=\max(0, -k)$，$H=\min(C, C-k)$

+ 当$y_1 = y_2$，两者同为1或同为-1，得：
  $$
  \alpha_1^{old}+\alpha_2^{old}=k
  $$
  所以$L=\max(0, k-C)$，$H=\min(C,k)$

<img src="/images/svm4.png" width="400px">

&emsp;&emsp;以上就确定了$\alpha_i$的边界。接下来要推到用于更新$\alpha$的迭代式，还是不失一般性地选择$\alpha_1$和$\alpha_2$来推导：
$$
\begin{aligned}
W(\alpha_2) &= \sum^n_{i=1}\alpha_i-\frac12\sum^n_{i=1}\sum^n_{j=1}y_iy_jx_i^Tx_j\alpha_i\alpha_j \\
&= \alpha_1 + \alpha_2 + \sum^n_{i=3}\alpha_i - \frac12\sum^n_{i=1}(\sum^2_{i=1}y_iy_jx_i^Tx_j\alpha_i\alpha_j+\sum^n_{j=3}y_iy_jx_i^Tx_j\alpha_i\alpha_j) \\
&= \alpha_1+\alpha_2+\sum^n_{i=3}\alpha_i-\frac12\sum^2_{i=1}(\sum^2_{j=1}y_iy_jx_i^Tx_j\alpha_i\alpha_j + \sum^n_{j=3}y_iy_jx_i^Tx_j\alpha_i\alpha_j)-\frac12\sum^n_{i=3}(\sum^2_{j=1}y_iy_jx_i^Tx_j\alpha_i\alpha_j+\sum^n_{j=3}y_iy_jx_i^Tx_j\alpha_i\alpha_j) \\
&= \alpha_1+\alpha_2+\sum^n_{i=3}\alpha_i-\frac12\sum^2_{i=1}\sum^2_{j=1}y_iy_j\alpha_i\alpha_jx_i^Tx_j - \sum^2_{i=1}\sum^n_{j=3}y_iy_j\alpha_i\alpha_jx_i^Tx_j - \frac12\sum^n_{i=3}\sum^n_{j=3}y_iy_j\alpha_i\alpha_jx_i^Tx_j \\
&= \alpha_1+\alpha_2 - \frac12\alpha_1^2x_1^Tx_1 - \frac12\alpha_2
^2x_2^Tx_2-y_1y_2\alpha_1\alpha_2x_1^Tx_2-y_1\alpha_1\sum^n_{j=3}\alpha_jy_jx_1^Tx_j - y_2\alpha_2\sum^n_{j=3}\alpha_jy_jx_2^Tx_j + \sum^n_{i=3}\alpha_i - \frac12\sum^n_{i=3}\sum^n_{j=3}y_iy_j\alpha_i\alpha_jx_i^Tx_j
\end{aligned}
$$
&emsp;&emsp;定义如下符号：
$$
\begin{aligned}
f(x_i) &= \sum^n_{j=1}\alpha_jy_jx_i^Tx_j+b \\
v_i &= \sum^n_{j=3}\alpha_jy_jx_i^Tx_j=f(x_i)-\sum^2_{j=1}\alpha_jy_jx_i^Tx_j-b
\end{aligned}
$$
&emsp;&emsp;所以替换符号后，得：
$$
W(\alpha_2) = \alpha_1+\alpha_2-\frac12\alpha_1^2x_1^Tx_1-\frac12\alpha_2^2x_2^Tx_2 - y_1y_2\alpha_1\alpha_2x_1^Tx_2 - y_1\alpha_1v_1 - y_2\alpha_2v_2 + constant
$$
&emsp;&emsp;因为我们不关心constant部分，对于$\alpha_1$和$\alpha_2$而言，其余都是常数项，求导时直接变为0。

&emsp;&emsp;然后我们通过$\alpha_1$和$\alpha_2$之间的约束关系，消去$\alpha_1$：
$$
\sum^n_{i=1}\alpha_iy_i=0 \\
\alpha_1y_1+\alpha_2y_2=-\sum^n_{i=3}\alpha_iy_i=B
$$
&emsp;&emsp;两边同乘$y_1$，得（利用$y_1^2=1$）：
$$
\alpha_1=By_1-y_1y_2\alpha_2=\gamma-s\alpha2
$$
&emsp;&emsp;其中，$\gamma=By$，$s=y_1y_2$。所以得到：
$$
W(\alpha_2) = \gamma-s\alpha_2+\alpha_2 - \frac12(r-s\alpha_2)^2x_1^Tx_1-\frac12\alpha_2^2x_2^Tx_2-s(\gamma-s\alpha_2)\alpha_2x_1^Tx_2 - y_1(\gamma-s\alpha_2)v_1-y_2\alpha_2v_2+constant
$$
&emsp;&emsp;对$W(\alpha_2)$求偏导：
$$
\frac{\partial W(\alpha_2)}{\partial \alpha_2} = -s + 1 + \gamma sx_1^Tx_1 - \alpha_2x_1^Tx_1-\alpha_2x_2^Tx_2-\gamma sx_1^Tx_2 + 2\alpha_2x_1^Tx_2 + y_2v_1 - y_2v_2 = 0 \\
\alpha_2^{new} = \frac{y_2(y_2-y_1+y_1\gamma(x_1^Tx_1-x_1^Tx_2)+v_1-v_2)}{(x_1^Tx_1+x_2^Tx_2-2x_1^Tx_2)}
$$
&emsp;&emsp;定义两个中间变量：

+ 误差项：$E_i=f(x_i) - y_i$
+ 学习率：$\eta=x_1^Tx_1+x_2^Tx_2-2x_1^Tx_2$

&emsp;&emsp;根据已知条件：
$$
\gamma = \alpha_1^{old} + s\alpha_2^{old} \\
v_j = \sum^n_{i=3}\alpha_iy_ix_j^Tx_i = f(x_j) - \sum^2_{i=1}\alpha_iy_ix_j^Tx_i - b
$$
&emsp;&emsp;代入上式求解：
$$
\begin{aligned}
\alpha_2^{new} &= \frac{y_2[(v_1-v_2+y_2-y_1) + (\alpha_1^{old}y_1+\alpha_2^{old}y_2)(x_1^Tx_1-x_1^Tx_2)]}{(x_1^Tx_1+x_2^Tx_2-2x_1^Tx_2)} \\
&= \frac{y_2(E_1-E_2)+\alpha_2^{old}(x_1^Tx_1+x_2^Tx_2-2x_1Tx_2)}{(x_1^Tx_1+x_2^Tx_2-2x_1^Tx_2)} \\
&= \alpha_2^{old} + \frac{y_2(E_1-E_2)}{\eta}
\end{aligned}
$$
&emsp;&emsp;详细推导过程：

<img src="/images/svm5.jpg" width="400px">

&emsp;&emsp;以上就是$\alpha_2$的更新迭代公式，考虑约束：$0 \lt \alpha_i \lt C$，作剪辑处理：
$$
\alpha_2^{new, clipped} = \begin{cases}
H \quad \quad \alpha_2^{new} \gt H \\
\alpha_2^{new} \quad \quad L \le \alpha_2^{new} \le H \\
L \quad \quad \alpha_2^{new} \lt L
\end{cases}
$$
&emsp;&emsp;根据：
$$
\begin{cases}
\alpha_1^{old} = \gamma - s\alpha_2^{old} \\
\alpha_1^{new} = \gamma - s\alpha_2^{new, clipped}\\
\end{cases}
$$
&emsp;&emsp;推出$\alpha_1$的迭代更新式：
$$
\alpha_1^{new} = \alpha_1^{old}+y_1y_2(\alpha_2^{old}-\alpha_2^{new, clipped})
$$
&emsp;&emsp;至此，我们已经推导出$\alpha_1$和$\alpha_2$的更新迭代式，然后考虑$b$的更新。

### 推导$b$

&emsp;&emsp;当$0 \lt \alpha_1^{new} \lt C$，该点是支持向量上的点，满足：
$$
y_1(w^Tx_1+b)=1
$$
&emsp;&emsp;两边同乘以$y_1$，得：
$$
w^Tx_1 + b = y_1 \\
\sum^n_{i=1}\alpha_iy_ix_ix_1 + b = y_1
$$
&emsp;&emsp;根据已经$\alpha_1$和$\alpha_2$去更新$b$，其余的$\alpha_i(i \ne1,2)$固定不变：
$$
b_1^{new}=y_1 - \sum^n_{i=3}\alpha_iy_ix_i^Tx_1 - \alpha_1^{new}y_1x_1^Tx_1-\alpha_2^{new}y_2x_2^Tx_1 \\
y_1 - \sum^n_{i=3}\alpha_iy_ix_i^Tx_1=-E_1+\alpha_1^{old}y_1x_1^Tx_1+ \alpha_2^{old}y_2x_2^Tx_1+b^{old}
$$
&emsp;&emsp;合并上面两条式子，得到：
$$
b_1^{new} = b^{old}-E_1-y_1(\alpha_1^{new} - \alpha_1^{old})x_1^Tx_1-y_2(\alpha_2^{new}-\alpha_2^{old})x_2^Tx_1
$$
&emsp;&emsp;同理，$b_2^{new}$推导为：
$$
b_2^{new} = b^{old}-E_2-y_1(\alpha_1^{new} - \alpha_1^{old})x_1^Tx_2-y_2(\alpha_2^{new}-\alpha_2^{old})x_2^Tx_2
$$
&emsp;&emsp;当$b_1$和 $b_2$都是有效时，它们是相等的：
$$
b^{new} = b_1^{new} = b_2^{new}
$$
&emsp;&emsp;当两个乘子在边界上，$b$和KKT条件一致；当不满足时，SMO算法选择它们的中点作为新的阈值：
$$
b =\begin{cases}
b_1 , \quad 0 \lt \alpha_1^{new} \lt C\\
b_2 , \quad 0 \lt \alpha_2^{new} \lt C\\
\frac{b_1+b_2}{2}, \quad otherwise
\end{cases}
$$

### 算法步骤总结

#### 第一步：计算误差$E_i$

$$
E_i=f(x_i) - y_i = \sum^n_{j=1}\alpha_jy_jx_i^Tx_j+b-y_i
$$

#### 第二步：计算$\alpha$上下界$L$和 $H$

$$
\begin{cases}
L=\max(0,\alpha_j^{old}-\alpha_i^{old}), H=\min(C, C+\alpha_j^{old}-\alpha_i^{old}), \quad \quad if\quad y_i \neq y_j\\
L=\max(0,\alpha_j^{old}+\alpha_i^{old}-C), H=\min(C, \alpha_j^{old}+\alpha_i^{old}), \quad \quad if \quad y_i = y_j\\
\end{cases}
$$

#### 第三步：计算$\eta$

$$
\eta = x_i^Tx_i+x_j^Tx_j-2x_i^Tx_j
$$

#### 第四步：更新$\alpha_j$

$$
\alpha_j^{new} = \alpha_j^{old} + \frac{y_j(E_i-E_j)}{\eta}
$$

#### 第五步：根据取值范围修剪$\alpha_j$

$$
\alpha_j^{new, clipped} = \begin{cases}
H \quad \quad \alpha_j^{new} \gt H \\
\alpha_j^{new} \quad \quad L \le \alpha_j^{new} \le H \\
L \quad \quad \alpha_j^{new} \lt L
\end{cases}
$$

#### 第六步：更新$\alpha_i$

$$
\alpha_i^{new} = \alpha_i^{old}+y_iy_j(\alpha_j^{old}-\alpha_j^{new, clipped})
$$

#### 第七步：更新$b_i$和$b_j$

$$
b_i^{new} = b^{old}-E_i-y_i(\alpha_i^{new} - \alpha_i^{old})x_i^Tx_i-y_j(\alpha_j^{new}-\alpha_j^{old})x_j^Tx_i \\
b_j^{new} = b^{old}-E_j-y_i(\alpha_i^{new} - \alpha_i^{old})x_i^Tx_j-y_j(\alpha_j^{new}-\alpha_j^{old})x_j^Tx_j
$$

#### 第八步：根据$b_i$和$b_j$更新$b$

$$
b =\begin{cases}
b_i , \quad 0 \lt \alpha_i^{new} \lt C\\
b_j , \quad 0 \lt \alpha_j^{new} \lt C\\
\frac{b_i+b_j}{2}, \quad otherwise
\end{cases}
$$

---

## 小结

&emsp;&emsp;如果你能够仔细阅读每一部分的推导，那么恭喜你已经基本掌握了SVM及SMO算法的原理。如果你是直接跳到这里的，那么我建议你静下心来慢慢看，我自己也是看了很多次才慢慢理解，这个过程是需要时间和耐心的。

&emsp;&emsp;以上就是SVM和SMO算法的主要内容，推导过程中如果有不严谨或者不正确的地方，欢迎大家指出！最后，谢谢您的支持！

---

## 手写笔记

| ![笔记1](/images/svm_notes1.jpg)   | ![笔记2](/images/svm_notes2.jpg)   |
| ---------------------------------- | ---------------------------------- |
| ![笔记3](/images/svm_notes3.jpg)   | ![笔记4](/images/svm_notes4.jpg)   |
| ![笔记5](/images/svm_notes5.jpg)   | ![笔记6](/images/svm_notes6.jpg)   |
| ![笔记7](/images/svm_notes7.jpg)   | ![笔记8](/images/svm_notes8.jpg)   |
| ![笔记9](/images/svm_notes9.jpg)   | ![笔记10](/images/svm_notes10.jpg) |
| ![笔记11](/images/svm_notes11.jpg) | ![笔记12](/images/svm_notes12.jpg) |
| ![笔记13](/images/svm_notes13.jpg) | ![笔记14](/images/svm_notes14.jpg) |
| ![笔记15](/images/svm_notes15.jpg) |                                    |

---

## 参考文献

1. [SMO by Platt](/download/SMO.pdf)

2. [SVM classnotes by Andrew Ng](/download/cs229-notes3.pdf)

3. [Class slides by Prof. Yan Pan](/download/ML-SVM.pdf)