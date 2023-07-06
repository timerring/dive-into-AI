# PyTorch: 计算图与动态图机制

## 计算图

计算图是用来描述运算的有向无环图

计算图有两个主要元素：

+ 结点 Node

+ 边 Edge

结点表示数据：如向量，矩阵，张量

边表示运算：如加减乘除卷积等

用计算图表示：y = (x+ w) * (w+1)
a = x + w
b = w + 1
y = a * b

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007140501247.png)

计算图与梯度求导

y = (x+ w) * (w+1)
a = x + w
b = w + 1
y = a * b

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007142316040.png)

$\begin{aligned}
\frac{\partial y}{\partial w} &=\frac{\partial y}{\partial a} \frac{\partial a}{\partial w}+\frac{\partial y}{\partial b} \frac{\partial b}{\partial w} \\
&=b * 1+a * 1 \\
&=b+a \\
&=(w+1)+(x+w) \\
&=2 * w+x+1 \\
&=2 * 1+2+1=5
\end{aligned}$

可见，对于变量w的求导过程就是寻找它在计算图中的所有路径的求导之和。

code：

```python
import torch

w = torch.tensor([1.], requires_grad=True)
x = torch.tensor([2.], requires_grad=True)

a = torch.add(w, x)     # retain_grad()
b = torch.add(w, 1)
y = torch.mul(a, b)

y.backward()
print(w.grad)
```

```
tensor([5.])
```

计算图与梯度求导
y = (x+ w) * (w+1)

叶子结点 ：用户创建的结点称为叶子结点，如 X 与 W

is_leaf:  指示张量是否为叶子结点

> 叶子节点的作用是标志存储叶子节点的梯度，而清除在反向传播过程中的变量的梯度，以达到节省内存的目的。
>
> 当然，如果想要保存过程中变量的梯度值，可以采用retain_grad()

grad_fn:  记录创建该张量时所用的方法（函数）

+ y.grad_fn= \<MulBackward0>
+ a.grad_fn= \<AddBackward0>
+ b.grad_fn= \<AddBackward0>

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007142938198.png)

## PyTorch的动态图机制

根据计算图搭建方式，可将计算图分为**动态图**和**静态图**

+ 动态图

  运算与搭建同时进行

  灵活 易调节

  例如动态图 PyTorch：

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007144304367.png)

+ 静态

  先搭建图， 后运算

  高效 不灵活。

  静态图 TensorFlow

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007144319338.png)