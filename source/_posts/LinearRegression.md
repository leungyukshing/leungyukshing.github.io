---
title: 线性回归--Linear Regression
tags:
  - Machine Learning
mathjax: true
abbrlink: 16617
date: 2019-03-14 18:52:59
---

## Intro

&emsp;&emsp;机器学习主要是两类问题，一类叫分类（Classification），另一个类叫回归（Regression）。简单来说，分类问题我们要预测的是一个类别（例如0，1，2这样的离散值），而回归问题我们需要预测的是一个实数（例如预测房价、预测身高这些连续型数据）。在这里我就来和大家分享回归算法中最简单，也是最重要的一种——线性回归（Linear Regression）。
<!-- more -->

## 原理

&emsp;&emsp;线性回归属于**有监督学习**的一种，线性回归模型接受一组特征参数，然后通过一定的优化算法，训练出最佳参数，最后对新输入的数据进行预测。

&emsp;&emsp;假定$x^{(i)}$为输入数据或输入特征（features），$y^{(i)}$为目标值（target），那么$(x^{(i)},y^{(i)})$就称为一个训练样例（training example）。模型接受的是一个包含若干个样例的训练集。

&emsp;&emsp;我们定义模型为一个线性模型：
$$
h_\theta(x)=\theta_0+\theta_1x_1+\dots
$$
&emsp;&emsp;在这里我们称$\theta_i$为参数，也叫权重。简单起见，我们可以写成以下形式：
$$
h(x) = \sum_{i=0}^n\theta_ix_i=\theta^Tx
$$
&emsp;&emsp;然后我们定义损失函数（loss function），这里使用常见的[最小均方法](http://leungyukshing.cn/archives/LMS.html)（LMS的原理、推导与优化均在这里）：
$$
J(\theta)=\frac{1}{2}\sum_{i=1}^m(h_\theta(x^{(i)} - y^{(i)}))^2&emsp;
$$

### Normal Equation

&emsp;&emsp;上面了解了线性回归的模型，我们可以从矩阵的角度去思考这个问题。实际上$min\;J(\theta)$等价于求解方程$h_\theta(x^{(i)})=y^{(i)}$。于是我们可以写出如下形式：
$$
\begin{cases}
\theta^Tx^{(1)} = h_\theta(x^{(1)}) = y^{(1)} \\
\theta^Tx^{(2)} = h_\theta(x^{(2)}) = y^{(2)} \\
\quad \quad \quad \quad \quad\vdots \\ \theta^Tx^{(m)} = h_\theta(x^{(m)}) = y^{(m)}\\
\end{cases}
$$
可以写成$X\theta=Y$，根据矩阵运算，最后得出：
$$
\theta = (X^TX)^{-1}X^TY
$$
&emsp;&emsp;使用矩阵的方法优缺点都很明显。

+ 优点：特征不需要做scaling，不需要选择学习率$\alpha$，不需要多次迭代，在数据集较小的时候具有比较高的运算效率
+ 缺点：当$n$很大的时候计算消耗很大，因为矩阵的运算复杂度是$O(n^3)$。同时，其最大的问题是**数值稳定性**，即$(X^TX)$的逆矩阵不一定存在，如果使用伪逆矩阵，其精度将会受很大影响。

### Local Weighted Linear Regression

&emsp;&emsp;局部加权线性回归，是一种解决过拟合和欠拟合问题的线性回归算法。由于过少的参数会导致**欠拟合现象**，而过多的参数会导致**过拟合**现象。而局部加权线性回归是一种**非参数学习方法**，也就是说模型每次预测新样本时都会重新训练临近数据而得到新的参数值。所以说，每次预测的数据需要依赖于训练集。它的优点有两个：

+ 一是使用加权的方式来确保依赖的数据是临近的，距离越近，关系越大
+ 二是它减少了较远数据的干扰，可以有效避免欠拟合现象。

&emsp;&emsp;它的区别在于$J(\theta)$：
$$
J(\theta) = \sum_iw^{(i)}(y^{(i)} - \theta^Tx^{(i)})^2
$$
&emsp;&emsp;其中$w^{(i)}$一般称为kernal，一个比较常见的做法是使用高斯分布：
$$
w^{(i)}=exp(-\frac{(x^{(i)} - x)^2}{2\gamma^2})
$$
&emsp;&emsp;从这个分布可以看出，参数$\theta$的选择过程中，距离新样本$x$越近的数据考虑的权重越大（实际上考虑的是他们的error）。

## 实战

&emsp;&emsp;这里将介绍使用Normal Equation的方法来实现。因为使用梯度下降的方法可以在后续的逻辑回归中详细介绍。

&emsp;&emsp;实战我选用了UCI数据集中预测鲍鱼年龄的数据。输入的是鲍鱼的一些特征，输出的是对鲍鱼年龄的预测。数据集地址：http://archive.ics.uci.edu/ml/datasets/Abalone

### 数据预处理

&emsp;&emsp;UCI下载的文件一般是txt，我们需要简单地处理一下这个原始数据。这一份鲍鱼的数据没有缺失，这省了不少功夫。但是我们需要将鲍鱼的性别从字符串类型转为数值类型。因为非常简单，并且本身就要分割数据，所以这里就不用one-hot，而是直接手动转换。建议这里数据预处理的代码和使用Regression的代码分开。

```python
import numpy as np
import pandas as pd

fr = open('./abalone.txt')
data = []
for line in fr.readlines():
    lineArr = []
    currLine = line.strip().split('\t')
    st = currLine[0].split(',')
    if st[0] == 'M':
        st[0] = 1
    elif st[0] == 'F':
        st[0] = -1
    else:
        st[0] = 0

    #print(st)
    for i in range(len(st)):
        lineArr.append(float(st[i]))
    data.append(lineArr)

#print(type(data))
dataMat = np.mat(data)
print(type(dataMat))
np.savetxt('abaloneProcessed.txt', dataMat, fmt='%.4f')
```

### 读入数据

&emsp;&emsp;按照txt的格式读入数据，注意数据类型问题。这里给出的代码也是读取UCI绝大多数数据集的一个模板，可以重复使用。

```python
def loadDataSet(fileName):
    numFeat = len(open(fileName).readline().split(' ')) - 1
    xArr = []
    yArr = []
    fr = open(fileName)
    for line in fr.readlines():
        lineArr = []
        currLine = line.strip().split(' ')
       #print(currLine)
        for i in range(numFeat):
            lineArr.append(float(currLine[i]))
        xArr.append(lineArr)
        yArr.append(float(currLine[-1]))
    return xArr, yArr
```

### 计算矩阵求解

&emsp;&emsp;使用Normal Equation的好处就在于代码编写简单。求解的过程实质上就是矩阵的运算，而这在python中是非常方便的。

```python
def computeRegression(xArr, yArr):
    xMat = np.mat(xArr)
    yMat = np.mat(yArr).T
    xTx = xMat.T * xMat
    if np.linalg.det(xTx) == 0.0:
        print('矩阵不存在逆矩阵')
        return
    ws = xTx.I * (xMat.T * yMat)
    return ws
```

### 定义Error

&emsp;&emsp;我们需要定义一个error来评估模型的好坏。这里用到了一个简单的均方。

```python
def rssError(yArr, yHatArr):
    return ((yArr - yHatArr) ** 2).sum()
```

### LWLR

&emsp;&emsp;这里的计算增加多了kernal矩阵，其余都是一样的。

```python
def lwlr(testPoint, xArr, yArr, k=1.0):
    xMat = np.mat(xArr)
    yMat = np.mat(yArr).T
    m = np.shape(xMat)[0]
    weights = np.mat(np.eye((m)))

    for j in range(m):
        diffMat = testPoint - xMat[j, :]
        weights[j, j] = np.exp(diffMat * diffMat.T / (-2.0 * k ** 2))
    xTx = xMat.T * (weights * xMat)

    if np.linalg.det(xTx) == 0.0:
        print('矩阵不存在逆矩阵')
        return
    ws = xTx.I * (xMat.T * (weights * yMat))
    return testPoint * ws
```

### LWLR测试

&emsp;&emsp;因为LWLR是非参数化学习方法，所以他的参数是动态的，我们需要另外写一个函数来进行测试。

```python
def lwTest(testArr, xArr, yArr, k=1.0):
    m = np.shape(testArr)[0]
    yHat = np.zeros(m)
    for i in range(m):
        yHat[i] = lwlr(testArr[i], xArr, yArr, k)
    return yHat
```

### main

&emsp;&emsp;这里提供了各种方法、参数之间的对比。

```python
if __name__ == '__main__':
    abX, abY = loadDataSet('abaloneProcessed.txt')
    print('训练集与测试集相同:局部加权线性回归,核k的大小对预测的影响:')
    yHat01 = lwTest(abX[0:99], abX[0:99], abY[0:99], 0.1)
    yHat1 = lwTest(abX[0:99], abX[0:99], abY[0:99], 1)
    yHat10 = lwTest(abX[0:99], abX[0:99], abY[0:99], 10)
    print('k=0.1时,误差大小为:',rssError(abY[0:99], yHat01.T))
    print('k=1  时,误差大小为:',rssError(abY[0:99], yHat1.T))
    print('k=10 时,误差大小为:',rssError(abY[0:99], yHat10.T))
    print('')
    print('训练集与测试集不同:局部加权线性回归,核k的大小是越小越好吗？更换数据集,测试结果如下:')
    yHat01 = lwTest(abX[100:199], abX[0:99], abY[0:99], 0.1)
    yHat1 = lwTest(abX[100:199], abX[0:99], abY[0:99], 1)
    yHat10 = lwTest(abX[100:199], abX[0:99], abY[0:99], 10)
    print('k=0.1时,误差大小为:',rssError(abY[100:199], yHat01.T))
    print('k=1  时,误差大小为:',rssError(abY[100:199], yHat1.T))
    print('k=10 时,误差大小为:',rssError(abY[100:199], yHat10.T))
    print('')
    print('训练集与测试集不同:简单的线性归回与k=1时的局部加权线性回归对比:')
    print('k=1时,误差大小为:', rssError(abY[100:199], yHat1.T))
    ws = computeRegression(abX[0:99], abY[0:99])
    yHat = np.mat(abX[100:199]) * ws
    print('简单的线性回归误差大小:', rssError(abY[100:199], yHat.T.A))
```

---

## 总结

&emsp;&emsp;线性回归问题是机器学习的基本问题之一，学习基本的线性回归算法是非常重要的。这里我特别分享了Normal Equation的做法。但是更为常见的是GD，这将在后续的博客中与大家分享。希望大家保持关注，谢谢！
