- [数据读取机制Dataloader与Dataset](#数据读取机制dataloader与dataset)
  - [DataLoader 与 Dataset](#dataloader-与-dataset)
    - [torch.utils.data.DataLoader](#torchutilsdatadataloader)
    - [区分Epoch、Iteration、Batchsize](#区分epochiterationbatchsize)
    - [torch.utils.data.Dataset](#torchutilsdatadataset)
  - [关于读取数据](#关于读取数据)


# 数据读取机制Dataloader与Dataset

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007201448681.png)

数据分为四个模块

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007201520132.png)

Sampler：生成索引

DataSet：根据索引读取图片及标签。

## DataLoader 与 Dataset

### torch.utils.data.DataLoader

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007201915039.png)

功能：构建可迭代的数据装载器

+ dataset : Dataset 类，决定数据从哪读取
  及如何读取
+ batchsize : 批大小
+ num_works : 是否多进程读取数据（减少时间，加速模型训练）
+ shuffle：每个 epoch 是否乱序
+ drop_last ：当样本数不能被 batchsize 整除时，是否舍弃最后一批数据

### 区分Epoch、Iteration、Batchsize

Epoch: 所有训练样本都已输入到模型中，称为一个 Epoch

Iteration：一批样本输入到模型中，称之为一个 Iteration

Batchsize：批大小，决定一个 Epoch 有多少个 Iteration

> 样本总数： 80 Batchsize 8
>
> 1 Epoch = 10 Iteration

> 样本总数： 87 Batchsize 8
>
> 1 Epoch = 10 Iteration？drop_last = True
>
> 1 Epoch = 11 Iteration？drop_last = False

### torch.utils.data.Dataset

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007202350534.png)

功能：

Dataset 抽象类，所有自定义的Dataset 需要继承它，并且复写\__getitem__()

getitem：接收一个索引，返回一个样本

## 关于读取数据

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007205351462.png)

通过debug详解数据的读取过程

DataLoader根据是否采用多进程，进入DataLoaderIter，使用Sampler获取index，再通过索引调用DatasetFetcher，在硬盘中读取imgandLabel，通过collate_fn整理成一个batchData。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221007205616471.png)