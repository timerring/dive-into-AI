- [autograd 自动求导系统](#autograd-自动求导系统)
  - [torch.autograd.backward](#torchautogradbackward)
  - [torch.autograd.grad](#torchautogradgrad)
- [逻辑回归 Logistic Regression](#逻辑回归-logistic-regression)
  - [逻辑回归](#逻辑回归)
  - [线性回归](#线性回归)
  - [对数回归](#对数回归)
- [机器学习模型训练步骤](#机器学习模型训练步骤)
- [逻辑回归的实现](#逻辑回归的实现)


## autograd 自动求导系统

torch.autograd

autograd

### torch.autograd.backward

`torch.autograd.backward ( tensors, grad_tensors=None,retain_graph=None,create_graph=False)`

功能：自动求取梯度

+ tensors : 用于求导的张量，如 loss
+ retain_graph : 保存计算图
+ create_graph：创建导数计算图，用于高阶求导
+ **grad_tensors ：多梯度权重(用于设置权重)**

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007151941349.png)

> 注意：张量类中的backward方法，本质上是调用的torch.autogtad.backward。

```python
    w = torch.tensor([1.], requires_grad=True)
    x = torch.tensor([2.], requires_grad=True)

    a = torch.add(w, x)
    b = torch.add(w, 1)
    y = torch.mul(a, b)

    y.backward(retain_graph=True) # 可以保存梯度图
    # print(w.grad)
    y.backward() # 可以求两次梯度
```

使用grad_tensors可以设置每个梯度的权重。

```python
    w = torch.tensor([1.], requires_grad=True)
    x = torch.tensor([2.], requires_grad=True)

    a = torch.add(w, x)     # retain_grad()
    b = torch.add(w, 1)

    y0 = torch.mul(a, b)    # y0 = (x+w) * (w+1)
    y1 = torch.add(a, b)    # y1 = (x+w) + (w+1)    dy1/dw = 2

    loss = torch.cat([y0, y1], dim=0)       # [y0, y1]
    grad_tensors = torch.tensor([1., 2.])

    loss.backward(gradient=grad_tensors) # gradient设置权重

    print(w.grad)
```

```
tensor([9.])
```

这个结果是由每一部分的梯度乘它对应部分的权重得到的。

### torch.autograd.grad

`torch.autograd.grad (outputs, inputs, grad_outputs=None,retain_graph= None, create_graph=False）`

功能：求取梯度

+ outputs : 用于求导的张量，如 loss
+ inputs : 需要梯度的 张量
+ create_graph: 创建导数计算图，用于高阶求导

+ retain_graph : 保存计算图
+ grad_outputs ：多梯度权重

```python
    x = torch.tensor([3.], requires_grad=True)
    y = torch.pow(x, 2)     # y = x**2

# grad_1 = dy/dx
    grad_1 = torch.autograd.grad(y, x, create_graph=True)
    print(grad_1)

# grad_2 = d(dy/dx)/dx
    grad_2 = torch.autograd.grad(grad_1[0], x, create_graph=True) 
    print(grad_2) # 求二阶导

    grad_3 = torch.autograd.grad(grad_2[0], x)
    print(grad_3)
    print(type(grad_3))
```

```
(tensor([6.], grad_fn=<MulBackward0>),)
(tensor([2.], grad_fn=<MulBackward0>),)
(tensor([0.]),)
<class 'tuple'>
```

注意：由于是元组类型，因此再次使用求导的时候需要访问里面的内容。

1.梯度不自动清零

```python
    w = torch.tensor([1.], requires_grad=True)
    x = torch.tensor([2.], requires_grad=True)

    for i in range(4):
        a = torch.add(w, x)
        b = torch.add(w, 1)
        y = torch.mul(a, b)

        y.backward()
        print(w.grad)
        # If not zeroed, the errors from each backpropagation add up.
        # This underscore indicates in-situ operation
        grad.zero_()
```

```
tensor([5.])
tensor([5.])
tensor([5.])
tensor([5.])
```

2.依赖于叶子结点的结点， requires_grad 默认为 True

```python
    w = torch.tensor([1.], requires_grad=True)
    x = torch.tensor([2.], requires_grad=True)

    a = torch.add(w, x)
    b = torch.add(w, 1)
    y = torch.mul(a, b)
# It can be seen that the attributes of the leaf nodes are all set to True
    print(a.requires_grad, b.requires_grad, y.requires_grad)
```

```
True True True
```

3.叶子结点不可执行 in place

> 什么是in place？
>
> 试比较：
>
> ```python
> a = torch.ones((1, ))
> print(id(a), a)
> 
> a = a + torch.ones((1, ))
> print(id(a), a)
> 
> a += torch.ones((1, ))
> print(id(a), a)
> # After executing in place, the stored address does not change
> ```
>
> ```
> 2413216666632 tensor([1.])
> 2413216668472 tensor([2.])
> 2413216668472 tensor([3.])
> ```
>
> 叶子节点不能执行in place，因为反向传播时会用到叶子节点张量的值，如w。而取值是按照w的地址取得，因此如果w执行inplace，则更换了w的值，导致反向传播错误。

## 逻辑回归 Logistic Regression

逻辑回归是线性的二分类模型

模型表达式：

$\begin{array}{c}
y=f(W X+b)\\
f(x)=\frac{1}{1+e^{-x}}
\end{array}$

 f(x)  称为Sigmoid函数，也称为Logistic函数

$\text { class }=\left\{\begin{array}{ll}
0, & 0.5>y \\
1, & 0.5 \leq y
\end{array}\right.$

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007163559451.png)

### 逻辑回归

$\begin{array}{c}
y=f(W X+b) \\
\quad=\frac{1}{1+e^{-(W X+b)}} \\
f(x)=\frac{1}{1+e^{-x}}
\end{array}$

线性回归是分析自变量 x 与 因变量 y( 标量 ) 之间关系的方法

逻辑回归是分析自变量 x 与 因变量 y( 概率 ) 之间关系的方法

逻辑回归也称为对数几率回归（等价）。

$\frac{y}{1-y}$表示对数几率。表示样本x为正样本的可能性。

> 证明等价：
>
> $\begin{array}{l}
> \ln \frac{y}{1-y}=W X+b \\
> \frac{y}{1-y}=e^{W X+b} \\
> y=e^{W X+b}-y * e^{W X+b} \\
> y\left(1+e^{W X+b}\right)=e^{W X+b} \\
> y=\frac{e^{W X+b}}{1+e^{W X+b}}=\frac{1}{1+e^{-(W X+b)}}
> \end{array}$

### 线性回归

自变量：X
因变量：y
关系：y=𝑊𝑋+𝑏

本质就是用WX+b拟合y。

### 对数回归

lny=𝑊𝑋+𝑏

就是用𝑊𝑋+𝑏拟合lny。

同理，对数几率回归就是用WX+b拟合对数几率。

## 机器学习模型训练步骤

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007164317211.png)

+ 数据采集，清洗，划分和预处理：经过一系列的处理使它可以直接输入到模型。
+ 模型：根据任务的难度选择简单的线性模型或者是复杂的神经网络模型。
+ 损失函数：根据不同的任务选择不同的损失函数，例如在线性回归中采用均方差损失函数，在分类任务中可以选择交叉熵。有了Loss就可以求梯度。
+ 得到梯度可以选择某一种优化方式，即优化器。采用优化器更新权值。
+ 最后再进行迭代训练过程。

## 逻辑回归的实现

```python
# -*- coding: utf-8 -*-

import torch
import torch.nn as nn
import matplotlib.pyplot as plt
import numpy as np
torch.manual_seed(10)


# ============================ step 1/5 Generate data ============================
sample_nums = 100
mean_value = 1.7
bias = 1
n_data = torch.ones(sample_nums, 2)
x0 = torch.normal(mean_value * n_data, 1) + bias      # 类别0 数据 shape=(100, 2)
y0 = torch.zeros(sample_nums)                         # 类别0 标签 shape=(100, 1)
x1 = torch.normal(-mean_value * n_data, 1) + bias     # 类别1 数据 shape=(100, 2)
y1 = torch.ones(sample_nums)                          # 类别1 标签 shape=(100, 1)
train_x = torch.cat((x0, x1), 0)
train_y = torch.cat((y0, y1), 0)


# ============================ step 2/5 Select Model ============================
class LR(nn.Module):
    def __init__(self):
        super(LR, self).__init__()
        self.features = nn.Linear(2, 1)
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        x = self.features(x)
        x = self.sigmoid(x)
        return x


lr_net = LR()   # Instantiate a logistic regression model


# ============================ step 3/5 Choose a loss function ============================
# Select the cross-entropy function for binary classification
loss_fn = nn.BCELoss()

# ============================ step 4/5 Choose an optimizer   ============================
lr = 0.01  # Learning rate
optimizer = torch.optim.SGD(lr_net.parameters(), lr=lr, momentum=0.9)

# ============================ step 5/5 model training ============================
for iteration in range(1000):

    # forward propagation
    y_pred = lr_net(train_x)

    # calculate loss
    loss = loss_fn(y_pred.squeeze(), train_y)

    # backpropagation
    loss.backward()

    # update parameters
    optimizer.step()

    # clear gradient
    optimizer.zero_grad()

    # drawing
    if iteration % 20 == 0:

        mask = y_pred.ge(0.5).float().squeeze()  # Classify with a threshold of 0.5
        correct = (mask == train_y).sum()  # Calculate the number of correctly predicted samples
        acc = correct.item() / train_y.size(0)  # Calculate classification accuracy

        plt.scatter(x0.data.numpy()[:, 0], x0.data.numpy()[:, 1], c='r', label='class 0')
        plt.scatter(x1.data.numpy()[:, 0], x1.data.numpy()[:, 1], c='b', label='class 1')

        w0, w1 = lr_net.features.weight[0]
        w0, w1 = float(w0.item()), float(w1.item())
        plot_b = float(lr_net.features.bias[0].item())
        plot_x = np.arange(-6, 6, 0.1)
        plot_y = (-w0 * plot_x - plot_b) / w1

        plt.xlim(-5, 7)
        plt.ylim(-7, 7)
        plt.plot(plot_x, plot_y)

        plt.text(-5, 5, 'Loss=%.4f' % loss.data.numpy(), fontdict={'size': 20, 'color': 'red'})
        plt.title("Iteration: {}\nw0:{:.2f} w1:{:.2f} b: {:.2f} accuracy:{:.2%}".format(iteration, w0, w1, plot_b, acc))
        plt.legend()

        plt.show()
        plt.pause(0.5)

        if acc > 0.99:
            break

```

实现一个逻辑回归步骤如上。后续会慢慢解释。