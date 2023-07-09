- [图像预处理 transforms](#图像预处理-transforms)
  - [transforms运行机制](#transforms运行机制)
  - [数据标准化transforms.normalize](#数据标准化transformsnormalize)
      - [transforms.Normalize](#transformsnormalize)


# 图像预处理 transforms

## transforms运行机制

torchvision：计算机视觉工具包

+ torchvision.transforms

  常用的图像预处理方法，例如：

  + 数据中心化
  + 数据标准化
  + 缩放
  + 裁剪
  + 旋转
  + 翻转
  + 填充
  + 噪声添加
  + 灰度变换
  + 线性变换
  + 仿射变换
  + 亮度、饱和度及对比度变换

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221008211408260.png)

+ torchvision.datasets

  常用数据集的 dataset 实现， MNIST CIFAR 10 ImageNet 等

+ torchvision.model

  常用的模型预训练， AlexNet VGG ResNet GoogLeNet 等

transforms运行的机制

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221008221435145.png)



## 数据标准化transforms.normalize

#### transforms.Normalize

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221008222508069.png)

标准化的含义是将数据的均值变为0，标准差变为1。

功能：逐channel 的对图像进行标准化

output = (input - mean) / std

+ mean ：各通道的均值
+ std ：各通道的标准差
+ inplace ：是否原地操作

对数据进行标准化后可以加快模型的收敛。通过比较不同的实验结果可知，一个好的数据分布更加利于模型的整体收敛。

