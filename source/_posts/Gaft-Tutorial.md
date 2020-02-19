---
title: GAFT框架使用
date: 2020-02-19 12:37:42
---

## Introduction for GAFT

&emsp;&emsp;GAFT（**G**enetic **A**lgorithm **F**ramework in **P**ython）是一个基于python的遗传算法框架，它提供了一系列的遗传运算子（operator）和一些插件接口（plugin interfaces），方便用户自定义。同时还提供了运行时分析（on-the-fly analysis）。GAFT还集成了MPI并行化接口，可以加速算法的运算。

<!-- more -->

## Installation

&emsp;&emsp;**Python环境**：Python 3.x

```bash
pip install gaft
```

&emsp;&emsp;如果希望支持并行化，需要安装MPI。

```bash
pip install mpi4py
```

## Quick Start

### 1.引入GAFT模块

```python
# components and operators
from gaft import GAEngine
from gaft.components import BinaryIndividual
from gaft.components import Population
from gaft.operators import RouletteWheelSelection
from gaft.operators import UniformCrossover
from gaft.operators import FlipBitMutation


# Analysis Plugin
from gaft.plugin_interfaces.analysis import OnTheFlyAnalysis


from gaft.analysis.fitness_store import FitnessStore

# Math
from math import sin, cos
```

+ GAFT的模块和遗传算法的结构有紧密的关系。`components`模块下的是个体和种群。`operators`下的是一个运算子，包括选择、交叉和变异。分析相关的插件是在`pluggin_terfaces`和`analysis`。

### 2.个体与种群的定义及初始化

```python
indv_template = BinaryIndividual(ranges=[(0, 10)], eps=0.001)
population = Population(indv_template=indv_template, size=20)
population.init()
```

+ 首先定义个体，这里用的是框架默认提供的`BinaryIndividual`。如果不符合我们的需求，也可以自定义。
+ 然后是定义种群，一般是传入个体的模板和种群的大小。
+ 最后是初始化种群，初始化种群时会逐个调用个体的初始化函数。

### 3.运算子的定义

```python
selection = RouletteWheelSelection()
crossover = UniformCrossover(pc=0.8, pe=0.5)
mutation = FlipBitMutation(pm=0.1)
```

+ 运算子在遗传算法中至关重要，运算中设计的好坏直接影响了种群的多样性，从而影响算法最后的结果。
+ 选择：GAFT提供了轮盘赌的选择运算。但是这里有一个bug，当所有个体的fitness都相同时，会报错，改进方法后面会讲。
+ 交叉：交叉运算是构成种群多样性的重要运算。这里用了UniformCrossover，里面有两个参数。`pc`指的是交叉的概率，推荐范围是`[0.25, 1.0]`，`pe`指的是选择到该个体后，每个位置上基因的交换概率，推荐范围是`[0.5, 0.7]`。
+ 变异：这里用的是FlipBitMutation，也就是按每个位上的基因进行翻转（0变为1，1变为0）。参数是`pm`指的是变异概率，推荐范围是`[0.001, 0.1]`

### 4.GA引擎初始化

```python
engine = GAEngine(population=population, selection=selection, crossover=crossover, mutation=mutation, analysis=[FitnessStore])
```

+ 把前面初始化的一系列个体、种群、运算子都加载到引擎中，包括分析插件。

### 5.注册适应值函数

```python
@engine.fitness_register
def fitness(indv):
    x, = indv.solution
    return x + 10*sin(5*x) + 7*cos(4*x)
```

+ 适应值函数是引导种群筛选掉差解，保留好解的重要工具。适应值高的个体我们认为是好解，所以在这里我们要做的就是把每个解的适应值计算出来。
+ 适应值函数需要注册到引擎，这里的写法是采用注解的方法。
+ 在这个任务中，我们计算的时候一个数学函数的最大值，所以在适应值计算中直接计算这个函数的值即可。

### 6. 运行时分析（Optional）

```python
@engine.analysis_register
class ConsoleOutput(OnTheFlyAnalysis):
    master_only = True
    interval = 1
    def register_step(self, g, population, engine):
        best_indv = population.best_indv(engine.fitness)
        msg = 'Generation: {}, best fitness: {:.3f}'.format(g, engine.fmax)
        engine.logger.info(msg)
```

+ 同样是采用注解的方式，将运行时分析的函数注册到引擎。里面需要实现`register_step`的方法。
+ 这里的分析内容是每一次迭代都打印最好的fitness。

### 7.运行

```python
engine.run(ng=100)
```

+ 运行算法，指定参数`ng`是迭代的次数。

### 8.分析结果

&emsp;&emsp;运行完后，会生成一个`best_fit.py`的文件，里面是每一轮迭代最好的个体以及对应的最好的fitness，通过这个文件我们能够追踪到整个种群的进化过程。

![GAFT result](/images/gaft0.png)

## Advance Usage

&emsp;&emsp;这一部分我加入了自定义的模块，把这个GAFT框架改造一下，比较符合我解题的需要。主要改动如下：

1. 自定义个体，直接使用chromosome，而跳过solution的编码过程，这样用起来更加直接。同时在初始化的时候加入新的参数。
2. 修复在轮盘赌时，所以个体fitness一样时出现的异常。



---

## Reference

1. [GAFT Github](https://github.com/PytLab/gaft)
2. [GAFT Documentation](https://gaft.readthedocs.io/en/latest/index.html)