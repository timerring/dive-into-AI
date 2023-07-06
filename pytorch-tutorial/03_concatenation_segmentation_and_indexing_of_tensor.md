## PyTorch: 张量的拼接、切分、索引

### 一、张量拼接与切分

#### 1.1 torch.cat

功能：将张量按维度dim 进行拼接

+ **tensors** : 张量序列

+ **dim**： 要拼接的维度

```python
 t = torch.ones((2, 3))

    t_0 = torch.cat([t, t], dim=0)
    t_1 = torch.cat([t, t, t], dim=1)

    print("t_0:{} shape:{}\nt_1:{} shape:{}".format(t_0, t_0.shape, t_1, t_1.shape))
```

```
t_0:tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]]) shape:torch.Size([4, 3])
t_1:tensor([[1., 1., 1., 1., 1., 1., 1., 1., 1.],
        [1., 1., 1., 1., 1., 1., 1., 1., 1.]]) shape:torch.Size([2, 9])
```

（2，3） -> （2，6） 

这里的dim维度与axis相同，0代表列，1代表行。

#### 1.2 torch.stack

功能：在新创建的维度 dim 上进行拼接（会拓宽原有的张量维度）

+ **tensors**：张量序列
+ **dim**：要拼接的维度

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006220254589.png)

```python
    t = torch.ones((2, 3))

    t_stack = torch.stack([t, t, t], dim=2)

    print("\nt_stack:{} shape:{}".format(t_stack, t_stack.shape))
```

可见，它在新的维度上进行了拼接。

参数[t, t, t]的意思就是在第n个维度上拼接成这个样子。

```
t_stack:tensor([[[1., 1., 1.],
         [1., 1., 1.],
         [1., 1., 1.]],

        [[1., 1., 1.],
         [1., 1., 1.],
         [1., 1., 1.]]]) shape:torch.Size([2, 3, 3])
# 在第二维度上进行了拼接
Process finished with exit code 0

```

#### 1.3 torch.chunk

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221006222011511.png)

功能：将张量按维度 dim 进行平均切分

返回值：张量列表

注意事项：若不能整除，最后一份张量小于其他张量。

+ input : 要切分的张量
+ chunks 要切分的份数
+ dim 要切分的维度

code

```python
    # cut into 3
    a = torch.ones((2, 7))  # 7
    list_of_tensors = torch.chunk(a, dim=1, chunks=3)   # 3

    for idx, t in enumerate(list_of_tensors):
        print("第{}个张量：{}, shape is {}".format(idx+1, t, t.shape))
```

可知，切分是7/3向上取整，每份是3，最后剩下的维度直接输出即可。

```
第1个张量：tensor([[1., 1., 1.],
        [1., 1., 1.]]), shape is torch.Size([2, 3])
第2个张量：tensor([[1., 1., 1.],
        [1., 1., 1.]]), shape is torch.Size([2, 3])
第3个张量：tensor([[1.],
        [1.]]), shape is torch.Size([2, 1])
```

#### 1.4 torch.split

`torch.split(Tensor, split_size_or_sections, dim)`

功能：将张量按维度 dim 进行切分

返回值：张量列表

+ tensor : 要切分的张量
+ split_size_or_sections 为 int 时，表示
  每一份的长度；为 list 时，按 list 元素切分
+ dim 要切分的维度

code：

```python
    t = torch.ones((2, 5))

    list_of_tensors = torch.split(t, [2, 1, 1], dim=1)  # [2 , 1, 2]
    for idx, t in enumerate(list_of_tensors):
        print("第{}个张量：{}, shape is {}".format(idx+1, t, t.shape))
```

是按照指定长度list进行切分的。注意list中长度总和必须为原张量在改维度的大小，不然会报错。

```
第1个张量：tensor([[1., 1., 1.],
        [1., 1., 1.]]), shape is torch.Size([2, 3])
第2个张量：tensor([[1., 1., 1.],
        [1., 1., 1.]]), shape is torch.Size([2, 3])
第3个张量：tensor([[1.],
        [1.]]), shape is torch.Size([2, 1])
```

### 二、张量索引

#### 2.1 torch.index_select

`torch.index_select(input, dim, index, out=None)`

功能：在维度dim 上，按 index 索引数据

返回值：依index 索引数据拼接的张量

+ input : 要索引的张量
+ dim 要索引的维度
+ index 要索引数据的序号

code：

```python
    t = torch.randint(0, 9, size=(3, 3))
    idx = torch.tensor([0, 2], dtype=torch.long)    # if float will report an error
    t_select = torch.index_select(t, dim=0, index=idx)
    print(idx)
    print("t:\n{}\nt_select:\n{}".format(t, t_select))
```

可见idx是一个存储序号的张量，而torch.index_select通过该张量索引原tensor并且拼接返回。

```
tensor([0, 2])
t:
tensor([[4, 5, 0],
        [5, 7, 1],
        [2, 5, 8]])
t_select:
tensor([[4, 5, 0],
        [2, 5, 8]])
```

#### 2.2 torch.masked_select

功能：按mask 中的 True 进行索引

返回值：一维张量(无法确定true的个数，因此也就无法显示原来的形状，因此这里返回一维张量)

+ input : 要索引的张量
+ mask 与 input 同形状的布尔类型张量

```python
    t = torch.randint(0, 9, size=(3, 3))
    mask = t.le(5)  # ge is mean greater than or equal/   gt: greater than  le  lt
    t_select = torch.masked_select(t, mask)
    print("t:\n{}\nmask:\n{}\nt_select:\n{} ".format(t, mask, t_select))
```

通过掩码来索引。

```
tensor([[4, 5, 0],
        [5, 7, 1],
        [2, 5, 8]])
mask:
tensor([[ True,  True,  True],
        [ True, False,  True],
        [ True,  True, False]])
t_select:
tensor([4, 5, 0, 5, 1, 2, 5]) 

Process finished with exit code 0
```

