# Python数据分析库介绍及引入惯例

## python的缺点

> Python有一个叫做全局解释器锁（Global Interpreter Lock，GIL）的组件，这是一种防止解释器同时执行多条Python字节码指令的机制。这并不是说Python不能执行真正的多线程并行代码。例如，Python的C插件使用原生的C或C++的多线程，可以并行运行而不被GIL影响，只要它们不频繁地与Python对象交互。

## 重要的python库

### NumPy

NumPy（Numerical Python的简称）是Python科学计算的基础包。

+ 快速高效的多维数组对象ndarray。

+ 作为在算法和库之间传递数据的容器。对于数值型数据，NumPy数组在存储和处理数据时要比内置的Python数据结构高效得多。此外，由低级语言（比如C和Fortran）编写的库可以直接操作NumPy数组中的数据，无需进行任何数据复制工作。

因此，许多Python的数值计算工具使用NumPy数组作为主要的数据结构。

### pandas

pandas提供了快速便捷处理结构化数据的大量数据结构和函数。用得最多的pandas对象

+ DataFrame，它是一个面向列（column-oriented）的二维表结构

+ Series，一个一维的标签化数组对象。

pandas兼具NumPy高性能的数组计算功能以及电子表格和关系型数据库（如SQL）灵活的数据处理功能。它提供了复杂精细的索引功能，能更加便捷地完成重塑、切片和切块、聚合以及选取数据子集等操作。

### matplotlib

matplotlib是最流行的用于绘制图表和其它二维数据可视化的Python库。

### SciPy

SciPy是一组专门解决科学计算中各种标准问题域的包的集合，主要包括下面这些包：

+ scipy.integrate：数值积分例程和微分方程求解器。

+ scipy.linalg：扩展了由numpy.linalg提供的线性代数例程和矩阵分解功能。

+ scipy.optimize：函数优化器（最小化器）以及根查找算法。

+ scipy.signal：信号处理工具。

+ scipy.sparse：稀疏矩阵和稀疏线性系统求解器。

+ scipy.special：SPECFUN（这是一个实现了许多常用数学函数（如伽玛函数）的Fortran库）的包装器。

+ scipy.stats：标准连续和离散概率分布（如密度函数、采样器、连续分布函数等）、各种统计检验方法，以及更好的描述统计法。

### scikit-learn

2010年诞生以来，scikit-learn成为了Python的通用机器学习工具包。

子模块包括：

+ 分类：SVM、近邻、随机森林、逻辑回归等等。

+ 回归：Lasso、岭回归等等。

+ 聚类：k-均值、谱聚类等等。

+ 降维：PCA、特征选择、矩阵分解等等。

+ 选型：网格搜索、交叉验证、度量。

+ 预处理：特征提取、标准化。

### statsmodels

一个统计分析包，包含经典统计学和经济计量学的算法。

+ 回归模型：线性回归，广义线性模型，健壮线性模型，线性混合效应模型等等。

+ 方差分析（ANOVA）。

+ 时间序列分析：AR，ARMA，ARIMA，VAR和其它模型。

+ 非参数方法： 核密度估计，核回归。

+ 统计模型结果可视化。

statsmodels更关注与统计推断，提供不确定估计和参数p-值。相反的，scikit-learn注重预测。

> 注意：当使用conda和pip二者安装包时，千万不要用pip升级conda的包，这样会导致环境发生问题。当使用Anaconda或Miniconda时，最好首先使用conda进行升级。

## 常见的引入惯例

最佳引入方式如下：

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import statsmodels as sm
```

> 不建议直接引入类似NumPy这种大型库的全部内容（from numpy import *）。


