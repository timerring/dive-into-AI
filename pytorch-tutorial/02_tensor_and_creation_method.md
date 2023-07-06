# PyTorch ： 了解Tensor(张量)及其创建方法

## 认识张量

张量是一个多维数组 ，它是标量、向量、矩阵的高维拓展。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006162704476.png)

比如说对于一张图片，它是3维张量，其中RGB就是其第三维张量。

### Tensor与 Variable

Variable是Pytorch的0.4.0版本之前的一个重要的数据结构，但是从0.4.0开始，它已经并入了Tensor中了。

Variable是 torch.autograd 中的数据类型，主要用于封装Tensor ，进行自动求导。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006163343217.png)

+ data: 被包装的 Tensor
+ grad: data 的梯度
+ grad_fn: 创建 Tensor 的 Function ，是自动求导的关键。比如说是加法还是乘法之类的。
+ requires_grad: 指示是否需要梯度，有些不需要梯度，设置为false可以节省内存。
+ is_leaf: 指示是否是叶子结点（张量）

### Tensor

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006163440529.png)

PyTorch0.4.0版开始， Variable 并入 Tensor

+ dtype: 张量的数据类型，如 torch.FloatTensor FloatTensor, torch.cuda.FloatTensor（cuda表示数据放在了GPU上）

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006163623925.png)

+ shape: 张量的形状，如 (64, 3, 224, 224）

+ device: 张量所在设备， GPU/CPU ，是加速的关键

## 张量的创建

### 一、直接创建

#### torch.tensor（）

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006163927858.png)

功能：从data 创建 tensor

+ data : 数据 , 可以是 list, numpy
+ dtype : 数据类型，默认与 data 的一致
+ device 所在设备 , cuda cpu
+ requires_grad ：是否需要梯度
+ pin_memory ：是否存于锁页内存

实例如下：

```python
import torch
import numpy as np

# Create tensors via torch.tensor

flag = True

if flag:
    arr = np.ones((3, 3))
    print("type of data:", arr.dtype)

    t = torch.tensor(arr, device='cuda')
    print(t)
```

```
type of data: float64
tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]], device='cuda:0', dtype=torch.float64)
```

其中，cuda表示采用了gpu，0是gpu的标号，由于只有一个gpu，因此是0。

#### torch.from_numpy(ndarray)

功能：从numpy 创建 tensor

**注意事项**：从 torch.from_numpy 创建的 tensor 于原 ndarray 共享内存 ，当修改其中一个的数据，另外一个也将会被改动。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006165313745.png)



实例代码：

```python
    # Create tensors via torch.from_numpy(ndarray)
    arr = np.array([[1, 2, 3], [4, 5, 6]])
    t = torch.from_numpy(arr)
    print("numpy array: ", arr)
    print("tensor : ", t)

    print("\n修改arr")
    arr[0, 0] = 0
    print("numpy array: ", arr)
    print("tensor : ", t)

    print("\n修改tensor")
    t[0, 0] = -1
    print("numpy array: ", arr)
    print("tensor : ", t)
    
```

通过结果可见，指向相同。

```
numpy array:  [[1 2 3]
 [4 5 6]]
tensor :  tensor([[1, 2, 3],
        [4, 5, 6]], dtype=torch.int32)

修改arr
numpy array:  [[0 2 3]
 [4 5 6]]
tensor :  tensor([[0, 2, 3],
        [4, 5, 6]], dtype=torch.int32)

修改tensor
numpy array:  [[-1  2  3]
 [ 4  5  6]]
tensor :  tensor([[-1,  2,  3],
        [ 4,  5,  6]], dtype=torch.int32)

```

### 二、依据数值创建

#### 2.1 torch.zeros（）

功能：依size 创建全 0 张量

+ size : 张量的形状 , 如 (3,3），(3，224，224）
+ out : 输出的张量
+ layout 内存中布局形式 , 有strided（默认）, sparse_coo（这个通常稀疏矩阵时设置，提高读取效率） 等
+ device 所在设备 , gpu cpu
+ requires_grad ：是否需要梯度

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006165756053.png)

code：

```python
    out_t = torch.tensor([1])

    t = torch.zeros((3, 3), out=out_t)

    print(t, '\n', out_t)
    print(id(t), id(out_t), id(t) == id(out_t))
```

可见，该out的值与t相同，因此out是一个输出的作用，将张量生成的数据赋值给另一个变量。

```
tensor([[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]]) 
 tensor([[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]])
2211683380904 2211683380904 True
```

#### 2.2 torch.zeros_like（）

功能：依据input 形状创建全 0 张量

+ intput : 创建与 input 同形状的全 0 张量
+ dtype : 数据类型
+ layout 内存中布局形式

#### 2.3 torch. ones()

#### 2.4 torch. ones_like（）

功能：input 形状创建全 1 张量

其他参数一样同上。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006170851588.png)

#### 2.5 torch. full()

#### 2.6 torch.full_like（）

功能：依据input 形状创建指定数据的张量

+ size : 张量的形状 , 如 (3,3)
+ fill_value : 张量的值

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006171018481.png)

code

```python
    t = torch.full((3, 3), 5)
    print(t)
```

```
tensor([[5, 5, 5],
        [5, 5, 5],
        [5, 5, 5]])
```

#### 2.7 torch. arange

功能：创建等差的1 维张量

注意事项：数值区间为[start,end）

+ start : 数列起始值
+ end : 数列“结束值”
+ step : 数列公差，默认为 1

code

```python
    t = torch.arange(2, 10, 2)
    print(t)
```

```
tensor([2, 4, 6, 8])
```

#### 2.8 torch. linspace

功能：创建均分的1 维张量

注意事项：数值区间为[start,end）

+ start : 数列起始值
+ end : 数列结束值
+ steps : 数列长度，注意是长度。

它的步长就是（end - start）/ steps。

```python
    t = torch.linspace(2, 10, 6)
    print(t)
```

```
tensor([ 2.0000,  3.6000,  5.2000,  6.8000,  8.4000, 10.0000])
```

#### 2.9 torch. logspace

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006172131505.png)

功能：创建对数均分的1 维张量

注意事项：长度为steps, 底为 base

+ start : 数列起始值
+ end : 数列结束值
+ steps : 数列长度
+ base : 对数函数的底，默认为 10

#### 2.10 torch. eye()

功能：创建单位对角矩阵（2 维张量）

注意事项：默认为方阵

+ n : 矩阵行数
+ m : 矩阵列数

### 三、依概率分布创建张量

#### 3.1 torch. normal()

功能：生成正态分布（高斯分布）

+ mean : 均值

+ std : 标准差

四种模式：
mean为标量， std为标量
mean为标量， std为张量
mean为张量， std为标量
mean为张量， std为张量

后三种基本用法相同，都是根据不同的维数进行

code：

```python
    # the mean and std both are tensors
    mean = torch.arange(1, 5, dtype=torch.float)
    std = torch.arange(1, 5, dtype=torch.float)
    t_normal = torch.normal(mean, std)
    print("mean:{}\nstd:{}".format(mean, std))
    print(t_normal)
```

由结果可知，其生成的tensor是上面每一维度的参数生成的。

```
mean:tensor([1., 2., 3., 4.])
std:tensor([1., 2., 3., 4.])
tensor([1.6614, 2.5338, 3.1850, 6.4853])
```

需要注意的是，对于mean和std都是标量的情况下，需要指定生成的size。

```python
    # mean: scalar std: scalar
    t_normal = torch.normal(0., 1., size=(4,))
    print(t_normal)
```

```
tensor([0.6614, 0.2669, 0.0617, 0.6213])
```

#### 3.2 torch. randn ()

#### 3.3 torch. randn_like ()

功能：生成标准正态分布（均值为0，方差为1）

size : 张量的形状。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006214135094.png)

#### 3.4 torch. rand()

#### 3.5 torch. rand_like

功能：在区间[0,1） 上，生成均匀分布

#### 3.6 torch. randint ()

#### 3.7 torch. randint_like ()

功能：区间[low, high) 生成整数均匀分布

size : 张量的形状

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006214253054.png)

#### 3.8 torch. randperm ()

功能：生成生成从0 到 n-1 的随机排列

n : 张量的长度

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006214407271.png)

#### 3.9 torch. bernoulli ()

功能 ：以 input 为概率，生成伯努力分布（0 1 分布，两点分布）

input : 概率值

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006214415134.png)