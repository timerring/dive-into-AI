- [神经网络入门](#神经网络入门)
  - [人工神经元](#人工神经元)
  - [多层感知机](#多层感知机)
  - [激活函数](#激活函数)
    - [Sigmoid（S型）](#sigmoids型)
    - [Tanh（双曲正切）](#tanh双曲正切)
    - [ReLU（修正线性单元）](#relu修正线性单元)
  - [反向传播（Back Propagation）](#反向传播back-propagation)
    - [前向传播](#前向传播)
    - [反向传播](#反向传播)
    - [梯度下降法（Gradient Decent）](#梯度下降法gradient-decent)
    - [学习率（Learning Rate）](#学习率learning-rate)
  - [损失函数（Loss Function）](#损失函数loss-function)
    - [损失函数(Loss Function)](#损失函数loss-function-1)
    - [代价函数 (Cost Function)](#代价函数-cost-function)
    - [目标函数(Objective Function)](#目标函数objective-function)
    - [两种常见损失函数](#两种常见损失函数)
      - [MSE](#mse)
      - [CE](#ce)
        - [softmax](#softmax)
  - [权值初始化（Initialization）](#权值初始化initialization)
    - [随机初始化法](#随机初始化法)
    - [自适应标准差](#自适应标准差)
  - [正则化（Regularization）](#正则化regularization)
    - [L2 Regularization](#l2-regularization)
    - [Dropout](#dropout)


# 神经网络入门

+ 神经网络与多层感知机：基础知识，激活函数、反向传播、损失函数、权值初始化和正则化
+ 卷积神经网络：统治图像领域的神经网络结构，发展历史、卷积操作和池化操作
+ 循环神经网络：统治序列数据的神经网络结构，RNN、GRU和LSTM

学习资源推荐

1. 吴恩达的深度学习课程: https://www.deeplearning.ai
2. 李宏毅的深度学习课程
3. 《deeplearning》俗称花书: https://github.com/exacity/deeplearningbook-chinese
4. 周志华的《机器学习》

## 人工神经元

Artificial Neural Unit

人工神经元： 人类神经元中抽象出来的数学模型

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230527224207853.png)

1904年研究出人类神经元，其中：

+ 树突：类似于input
+ 细胞核：类似于operation ：处理操作+激活函数
+ 轴突末梢：类似于output

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230527224610266.png)

1943年心理学家W.S. McCulloch和数理逻辑学家W.Pitts研究出人工神经元,称为M-Р模型。
$$
f(\sum_{i=1}^{N} I_{i} \cdot W_{i})=y
$$
人工神经网络： 大量神经元以某种连接方式构成的机器学习模型

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230527224941075.png)

第一个神经网络：1958年，计算机科学家Rosenblatt提出的Perceptron（感知机）
$$
o=\sigma(\langle\mathbf{w},\mathbf{x}\rangle+b)
$$


$$
\sigma(x)=\{\begin{array}{ll}
1 & \text { if } x>0 \\
0 & \text { otherwise }
\end{array}
$$

引发了第一波神经网络的热潮，但感知机的致命缺点是：Minsky在1969年证明Perceptron无法解决异或问题。根源在于，二维层面上神经网络是一条直线。无法划分异或的区间。


$$
\begin{array}{rlr}
0 & =\sigma(\mathrm{x}_{0} \mathrm{w}_{0}+\mathrm{x}_{1} \mathrm{w}_{1}+\mathrm{b}) \\
0 & =\mathrm{x}_{0} \mathrm{w}_{0}+\mathrm{x}_{1} \mathrm{w}_{1}+\mathrm{b} \\
\mathrm{x}_{1} \mathrm{w}_{1} & =0-\mathrm{x}_{0} \mathrm{w}_{0}-\mathrm{b} \\
\mathrm{x}_{1} & =  -\frac{\mathrm{w}_{0}}{\mathrm{w}_{1}} \mathrm{x}_{0}-\frac{\mathrm{b}}{\mathrm{w}_{1}} \\
\mathrm{y} & =  \mathrm{kx}+\mathrm{b}
\end{array}
$$


![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528223959705.png)

## 多层感知机

多层感知机(Multi Layer Perceptron,MLP):单层神经网络基础上引入一个或多个隐藏层,使神经网络有多个网络层，因而得名多层感知机。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528224416573.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528224528305.png)

隐藏层的输入权重矩阵是  $\mathrm{W}_{4 \times 5}$ 

隐藏层的权重矩阵是  $W_{5 \times 3} $

前向传播:（以下方便起见省略了偏置）


$$
\begin{array}{l}
\sigma(\mathrm{X}_{1 \times 4} \cdot \mathrm{W}_{\mathrm{h}})=\mathrm{H}_{1 \times 5} \\
\sigma(\mathrm{H}_{1 \times 5} \cdot \mathrm{W}_{\mathrm{o} \times 3})=\mathrm{O}_{1 \times 3}
\end{array}
$$


整个过程如果没有激活函数，网络退化为单层网络，下面证明：


$$
\begin{aligned}
\boldsymbol{H} & =\boldsymbol{X} \boldsymbol{W}_{\mathrm{h}}+\boldsymbol{b}_{\mathrm{h}} \\
\boldsymbol{O} & =\boldsymbol{H} \boldsymbol{W}_{\mathrm{o}}+\boldsymbol{b}_{\mathrm{o}} \\
\boldsymbol{O} & =(\boldsymbol{X} \boldsymbol{W}_{\mathrm{h}}+\boldsymbol{b}_{\mathrm{h}}) \boldsymbol{W}_{\mathrm{o}}+\boldsymbol{b}_{\mathrm{o}}=\boldsymbol{X} \boldsymbol{W}_{\mathrm{h}} \boldsymbol{W}_{\mathrm{o}}+\boldsymbol{b}_{\mathrm{h}} \boldsymbol{W}_{\mathrm{o}}+\boldsymbol{b}_{\mathrm{c}}
\end{aligned}
$$


可见，最终网络还是可以化简替代，退化为XW+b的单层网络。

当隐藏层加入**激活函数，可避免网络退化**


$$
\begin{array}{l}
\mathbf{h}=\sigma(\mathbf{W}_{1} \mathbf{x}+\mathbf{b}_{1}) \\
\mathbf{0}=\mathbf{w}_{2}^{\mathrm{T}} \mathbf{h}+\mathbf{b}_{2}
\end{array}
$$



## 激活函数

作用：

1. 让多层感知机成为真正的多层，否则等价于一层；
2. 引入非线性，使网络可以逼近任意非线性函数（详见：**万能逼近定理**，universal approximator）

激活函数需要具备以下几点性质:

1. **连续并可导**（允许少数点上不可导），便于利用数值优化的方法来学习网络参数
2. 激活函数及其导函数要尽可能的简单，有利于提高网络计算效率
3. 激活函数的导函数的**值域**要在**合适**区间内，反向传播中会使用到激活函数的导函数，数值会影响到权重的更新，不能太大也不能太小，否则会影响训练的效率和稳定性。

**常见激活函数**：Sigmoid（S型） ， Tanh（双曲正切）， ReLU（修正线性单元）

### Sigmoid（S型）

常用于RNN以及二分类中。做二分类的输出的激活函数；做循环神经网络中门控单元的激活函数。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528225633959.png)

有饱和区，导函数的饱和区（神经元的值差不多，梯度差不多），更新困难。
$$
\begin{array}{c}
g(z)=\frac{1}{1+e^{-z}} \\
g^{\prime}(z)=g(z) *(1-g(z))
\end{array}
$$
### Tanh（双曲正切）

特点，零均值。同样存在**饱和区**，不利于梯度传播。中间的是**线性区**。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528225717490.png)
$$
\begin{array}{c}
\tanh (x)=\frac{e^{x}-e^{-x}}{e^{x}+e^{-x}} \\
g^{\prime}(z)=1-(g(z))^{2}
\end{array}
$$
### ReLU（修正线性单元）

不存在饱和区，卷积神经网络中经常使用。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230528225751968.png)
$$
\begin{array}{c}
\text { Relu }=\max (0, x) \\
g^{\prime}(z)=\{\begin{array}{ll}
1 & \text { if } \mathrm{z}>0 \\
\text { undefined } & \text { if } \mathrm{z}=0 \\
0 & \text { if } \mathrm{z}<0
\end{array}.
\end{array}
$$

## 反向传播（Back Propagation）

### 前向传播

输入层数据从前向后，数据逐步传递至输出层
$$
\begin{array}{l}
\mathrm{z}=\mathrm{x} \cdot \mathrm{W}^{(1)} \\
\mathrm{h}=\phi(\mathrm{z}) \\
\mathrm{O}=\mathrm{h} \cdot \mathrm{W}^{(2)}
\end{array}
$$
### 反向传播

**损失函数**开始从后向前，**梯度**逐步传递至一层

反向传播作用：用于权重更新，使网络输出更接近标签

损失函数：衡量模型输出与真实标签的差异，$Loss=f(\hat{y}, y)$

反向传播原理：微积分中的链式求导法则
$$
\mathrm{y}=\mathrm{f}(\mathrm{u}), \mathrm{u}=\mathrm{g}(\mathrm{x}) \quad \frac{\partial \mathrm{y}}{\partial \mathrm{x}}=\frac{\partial \mathrm{y}}{\partial \mathrm{u}} \frac{\partial \mathrm{u}}{\partial \mathrm{x}}
$$


![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230529132846315.png)

其中，$\phi(Z)$ 代表激活函数。$L$代表损失函数。


$$
\begin{array}{l}
\frac{\partial L}{\partial W^{(2)}}=\operatorname{prod}(\frac{\partial L}{\partial O}, \frac{\partial O}{\partial W^{(2)}})=\frac{\partial L}{\partial O} \cdot h^{\top} \\
\frac{\partial L}{\partial h}=\operatorname{prod}(\frac{\partial L}{\partial O}, \frac{\partial O}{\partial h})=W^{(2)\top} \cdot \frac{\partial L}{\partial O} \\
\frac{\partial L}{\partial z}=\operatorname{prod}(\frac{\partial L}{\partial O}, \frac{\partial O}{\partial h}, \frac{\partial h}{\partial z})=\frac{\partial L}{\partial h} \odot \phi^{\prime}(Z) \\
\frac{\partial L}{\partial W^{(1)}}=\operatorname{prod}(\frac{\partial L}{\partial O}, \frac{\partial O}{\partial h}, \frac{\partial h}{\partial z}, \frac{\partial z}{\partial W^{(1)}})=\frac{\partial L}{\partial z} \cdot X^{\top}
\end{array}
$$
其中，prod(x,y) 表示x与y根据形状做必要的交换，然后相乘。$\odot$表示逐元素相乘。通过上述式子，可以容易发现，存在链式关系，即后一项的梯度可以用于前一项的梯度上。

**从Loss出发有多少条通路，梯度就有多少项，进行相加。上图所示，只有一条通路。用蓝笔画的圆圈，则是两条通路。**

### 梯度下降法（Gradient Decent）

**梯度下降法**（Gradient Decent）：权值沿梯度**负方向更新**，使函数值减小

导数：函数在**指定坐标轴上**的变化率

方向导数：指定方向上的变化率（在多维空间）

梯度：一个向量，方向为方向导数取得最大值的方向。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230529133827958.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230529133853457.png)

### 学习率（Learning Rate）

**学习率**（Learning Rate）：控制更新步长

沿梯度**负方向更新**：
$$
\boldsymbol{w}_{\mathrm{i}+1}=\boldsymbol{w}_{\mathrm{i}}-\boldsymbol{g}(\boldsymbol{w}_{\mathrm{i}})
$$
例：
$$
\begin{array}{l}

\mathrm{y}=\mathrm{f}(\mathrm{x})=4 * \mathrm{x}^{2} \\
\mathrm{y}^{\prime}=\mathrm{f}^{\prime}(\mathrm{x})=8 * \mathrm{x} \\
\mathrm{x}_{0}=2, \quad \mathrm{y}_{0}=16, \mathrm{f}^{\prime}(\mathrm{x}_{0})=16 \\
\mathrm{x}_{1}=\mathrm{x}_{0}-\mathrm{f}^{\prime}(\mathrm{x}_{0})=2-16=-14 \\
\mathrm{x}_{1}=-14, \quad \mathrm{y}_{1}=784, \mathrm{f}^{\prime}(\mathrm{x}_{1})=-112 \\
\mathrm{x}_{2}=\mathrm{x}_{1}-\mathrm{f}^{\prime}(\mathrm{x}_{1})=-14+112=98, \quad \mathrm{y}_{2}=38416 \\
\end{array}
$$
对比：

无学习率：$\boldsymbol{w}_{\mathrm{i}+1}=\boldsymbol{w}_{\mathrm{i}}-\boldsymbol{g}(\boldsymbol{w}_{\mathrm{i}})$

有学习率：$\boldsymbol{w}_{\mathrm{i}+1}=\boldsymbol{w}_{\mathrm{i}}- LR * \boldsymbol{g}(\boldsymbol{w}_{\mathrm{i}})$

不同学习率的影响：分别为1——0.25——0.24——0.1

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230529134435694.png)

## 损失函数（Loss Function）

损失函数：衡量模型输出与真实的标签之间的差距

### 损失函数(Loss Function)

描述的单样本的差异值
$$
\operatorname{Loss}=\mathrm{f}(\hat{\mathrm{y}}, \mathrm{y})
$$

### 代价函数 (Cost Function)

描述的是总体样样本\整个数据集的Loss的平均值 
$$
cost=\frac{\mathbf{1}}{\mathrm{N}} \sum_{i}^{N} f(y_{i}^{\wedge}, y_{i})
$$
### 目标函数(Objective Function)

模型输出与标签之间的差异+正则项(控制模型复杂度防止过拟合现象)
$$
Obj= Cost + Regularization Term
$$

### 两种常见损失函数

#### MSE

MSE (均方误差, Mean Squared Error)：输出与标签之差的平方的均值，常在**回归任务**中使用。计算公式：
$$
\mathrm{MSE}=\frac{\sum_{\mathrm{i}=1}^{\mathrm{n}}(\mathrm{y}_{\mathrm{i}}-\mathrm{y}_{\mathrm{i}}^{\mathrm{p}})^{2}}{\mathrm{n}}
$$
公式中p代表predict。

> 例: label  =(1,2)  pred  =(1.5,1.5) 
> $$
> \mathrm{MSE}=\frac{[(1-1.5)^{2}+(2-1.5)^{2}]}{2}=0.25
> $$

#### CE

CE（Cross Entropy，交叉熵) ：交叉熵源自信息论，用于衡量两个**分布**的差异，常在**分类任务**中使用。计算公式:
$$
\mathrm{H}(\mathrm{p}, \mathrm{q})=-\sum_{\mathrm{i}=1}^{\mathrm{n}} \mathrm{p}(\mathrm{x}_{\mathrm{i}}) \log \mathrm{q}(\mathrm{x}_{\mathrm{i}})
$$
p是指真是分布，q是模型的分布，试图用q去逼近p。分布之间的距离是没有对应关系的。在给定 p 的情况下，如果 q 和 p 越接近，交叉熵越小；如果 q 和 p 越远，交叉熵就越大。

+ **自信息**：$I(x)=−logP(x)$ , p(x) 是某事件发生的概率

+ **信息熵**：描述信息的不确定度，信息熵越大信息越不确定，信息熵越小

  **信息熵** = 所有可能取值的信息量的期望，即

$$
\mathrm{H}(\mathrm{x})=\mathrm{E}_{\mathrm{x}-\mathrm{p}}[\mathrm{I}(\mathrm{x})]=-\mathrm{E}[\log \mathrm{P}(\mathrm{x})]=-\sum_{\mathrm{i}=1}^{\mathrm{N}} \mathrm{p}_{\mathrm{i}} \log (\mathrm{p}_{\mathrm{i}})
$$

+ **相对熵**：又称**K-L散度**，**衡量两个分布之间的差异**。用概率分布 q 来近似 p 时所造成的信息损失量．KL 散度是按照概率分布q的最优编码对真实分布为p的信息进行编码，其平均编码长度（即交叉熵）𝐻(p, q) 和 p 的最优平均编码长度（即熵）𝐻(p) 之间的差异．
  $$
  \begin{aligned}
  \mathrm{D}_{\mathrm{KL}}(\mathrm{P} \| \mathrm{Q})=\mathrm{E}_{\mathrm{x} \sim \mathrm{p}}[\log \frac{\mathrm{P}(\mathrm{x})}{\mathrm{Q}(\mathrm{x})}] & =\mathrm{E}_{\mathrm{x}-\mathrm{p}}[\log \mathrm{P}(\mathrm{x})-\log \mathrm{Q}(\mathrm{x})] \\
  & =\sum_{\mathrm{i}=1}^{\mathrm{N}} \mathrm{P}(\mathrm{x}_{\mathrm{i}})(\log \mathrm{P}(\mathrm{x}_{\mathrm{i}})-\log \mathrm{Q}(\mathrm{x}_{\mathrm{i}}))
  \end{aligned}
  $$

由上式可知，$\mathrm{H}(\mathrm{p}, \mathrm{q})=\mathrm{H}(\mathrm{P})+\mathrm{D}_{-} \mathrm{KL}(\mathrm{P} \| \mathrm{Q})$ ,即 **交叉熵 = 信息熵 + 相对熵**。

又因为信息熵是一个关于p（真实分布）的常数，因此**优化交叉熵等价于优化相对熵**。

> 举例：计算损失函数（分类任务）
>
> ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626164444697.png)
> $$
> \begin{array}{l}
> \mathrm{H}(\mathrm{p}, \mathrm{q})=-\sum_{\mathrm{i}=1}^{\mathrm{n}} \mathrm{p}(\mathrm{x}_{\mathrm{i}}) \log \mathrm{q}(\mathrm{x}_{\mathrm{i}})\\
> \operatorname{loss}=-(0^{\star} \log (0.05)+0^{\star} \log (0.1)+1^{\star} \log (0.7)+0^{\star} \log (0.15))=0.36
> \end{array}
> $$

这里predict应该是一个概率分布的形式，模型输出如果想化为概率的形式，可以采用softmax函数。

##### softmax

Softmax函数可以将多个标量映射为一个概率分布。（将数据变换到符合概率分布的形式）

> 概率有两个性质：
>
> 1. 概率值是非负的
> 2. 概率之和等于1

$$
\mathrm{y}_{\mathrm{i}}=\mathrm{S}(\boldsymbol{z})_{\mathrm{i}}=\frac{\mathrm{e}^{\mathrm{z}_{\mathrm{i}}}}{\sum_{\mathrm{j}=1}^{\mathrm{C}} \mathrm{e}^{\mathrm{z}_{\mathrm{j}}}}, \mathrm{i}=1, \ldots, \mathrm{C}
$$

| 概率           | Softmax                   |
| -------------- | ------------------------- |
| 概率值是非负的 | 取指数，实现非负          |
| 概率之和等于1  | 除以指数之和，实现之和为1 |

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626165409071.png)

> 没有一个适合所有任务的损失函数，损失函数设计会涉及算法类型、求导是否容易、数据中异常值的分布等问题。
>
> 更多损失函数可到PyTorch网站：https://pytorch.org/docs/stable/nn.html#loss-functions
>
> 函数解读： https://zhuanlan.zhihu.com/p/61379965

## 权值初始化（Initialization）

**权值初始化**：训练前对权值参数赋值，良好的权值初始化有利于模型训练

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626214827685.png)

> 简便但错误的方法：初始化为全0——使网络层退化，多少个神经元都等于一个神经元，没有训练的意义。

### 随机初始化法

高斯分布随机初始化，从高斯分布中随机采样，对权重进行赋值，比如 N～（0，0.01）

> ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626215004326.png)
>
> 3$\sigma$准则: 数值分布在（μ-3$\sigma$,μ+3$\sigma$)中的概率为99.73%

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626215027525.png)

控制权值的尺度：不能让权值太小，如果权值过小，相当于网络退化。例如如果权值过大，会使得一些值落入sigmoid函数中的饱和区域，饱和区域中的梯度接近于0，使梯度消失，不利于模型的训练。

### 自适应标准差

自适应方法随机分布中的标准差

+ Xavier初始化：《Understanding the difficulty of training deep feedforward neural networks 》2010
  $$
  \mathrm{U}(-\sqrt{\frac{6}{a+b}}, \sqrt{\frac{6}{a+b}})
  $$
  a是输入神经元的个数，b是输出神经元的个数。假设$-\sqrt\frac{6}{a+b}$为A，$\sqrt\frac{6}{a+b}$为B，则可知$\frac{A+B}{2}=mean=0$，$std=\sqrt\frac{(B-A)^2}{12}$。

+ Kaiming初始化/MSRA：《Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification》2015

## 正则化（Regularization）

**Regularization**：减小**方差**的策略，通俗理解为**减轻过拟合**的策略

误差可分解为：偏差，方差与噪声之和。即**误差 = 偏差 + 方差 + 噪声**之和（西瓜书）

+ **偏差**度量了学习算法的期望预测与真实结果的偏离程度，即刻画了学习算法本身的拟合能力
+ **方差**度量了同样大小的训练集的变动所导致的学习性能的变化，即刻画了数据扰动所造成的影响
+ **噪声**则表达了在当前任务上任何学习算法所能达到的期望泛化误差的下界

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626233115981.png)

**过拟合现象**：方差过大；在训练集表现良好，在测试集表现糟糕。

损失函数(Loss Function)：描述的单样本的差异值
$$
\text { Loss }=\mathrm{f}(\hat{\mathrm{y}}, \mathrm{y})
$$
代价函数 (Cost Function)：描述的是总体样样本\整个数据集的Loss的平均值
$$
\text { Cost }=\frac{1}{N} \sum_{i}^{N} \mathrm{f}({\hat{y_{i}}}, \mathrm{y}_{\mathrm{i}})
$$
目标函数(Objective Function)：模型输出与标签之间的差异+正则项（约束）控制模型复杂度防止过拟合现象。

$Obj= Cost + Regularization Term$

L1 Regularization Term:  $\sum_{\mathrm{i}}^{\mathrm{N}}|\mathrm{w}_{\mathrm{i}}|$

L2 Regularization Term:  $\sum_{\mathrm{i}}^{\mathrm{N}} \mathrm{w}_{\mathrm{i}}^{2} $

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626234254641.png)

两个权值w1和w2组成的权值等高线，在所有等高线中选择L2正则化最小的（即圆与椭圆相切），L1正则化最小的（即方框与椭圆相切）。

> 参考深度学习花书 第七章

### L2 Regularization

**weight decay** (权值衰减)L2正则化含有权值衰减的作用。

目标函数(Objective Function):
$$
\begin{array}{l}
\boldsymbol{O} \boldsymbol{b} \boldsymbol{j}=\text { textCost }+ \text { Regularization Term } \\
\boldsymbol{O} \boldsymbol{b} \boldsymbol{j}=\boldsymbol{L} \boldsymbol{o s s}+\frac{\lambda}{2} * \sum_{\mathrm{i}}^{\mathrm{N}} \boldsymbol{w}_{\mathrm{i}}^{2}
\end{array}
$$
无正则项：
$$
\mathrm{w}_{\mathrm{i}+1}=\mathrm{w}_{\mathrm{i}}-\frac{\partial \mathrm{obj}}{\partial \mathrm{w}_{\mathrm{i}}}=\mathrm{w}_{\mathrm{i}}-\frac{\partial \text { Loss }}{\partial \mathrm{w}_{\mathrm{i}}}
$$
有正则项: 
$$
\begin{aligned}
\mathrm{w}_{\mathrm{i}+1}=\mathrm{w}_{\mathrm{i}}-\frac{\partial \mathrm{obj}}{\partial \mathrm{w}_{\mathrm{i}}}=\mathrm{w}_{\mathrm{i}} & -(\frac{\partial \mathrm{Loss}}{\partial \mathrm{w}_{\mathrm{i}}}+\lambda^{*} \mathrm{w}_{\mathrm{i}}) \\
& =\mathrm{w}_{\mathrm{i}}(1-\lambda)-\frac{\partial \mathrm{Loss}}{\partial \mathrm{w}_{\mathrm{i}}}
\end{aligned}
$$
这里$\lambda$相当于一个系数, $0<\lambda<1$，看更关注Loss还是更关注正则项的一个调节系数。通过有无正则项的对比，可以看出L2正则化确实起到了衰减权值的作用。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626235108454.png)

### Dropout

**Dropout**：随机失活

**优点**：避免过度依赖某个神经元，实现减轻过拟合

**随机**：dropout probability （eg：p=0.5）

**失活**：weight = 0

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230626235633123.png)

注意事项：训练和测试两个阶段的**数据尺度变化**，例如测试的时候会使用去全部的神经元，而在训练的时候会dropout一部分神经元。

测试时，神经元输出值需要乘以P（概率：一定概率丢失神经元）

> 例如：
>
> 测试时数据规模是100：$\sum_{i=1}^{100} W_{i} \cdot x_{i}=100 \times P_{p}=50$ 这时需要乘一个随即失活的概率$P_{p}$，使数据规模与训练时一致。
>
> 训练时数据规模是50：$\sum_{i=1}^{50} W_{i} \cdot x_{i}=100 \times 0.5=50$ 这里有50%概率$P_{p}$ dropout每个神经元。

其他正则化方法：BN, LN, IN, GN.

+ Batch normalization： 《Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift》
+ Layer Normalization： 《Layer Normalization》
+ Instance Normalization： 《Instance Normalization: The Missing Ingredient for Fast Stylization》
+ Group Normalization：《Group Normalization》
